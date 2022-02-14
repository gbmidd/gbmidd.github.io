---
layout: single
title:  "Run Arbitrary Code on a Node server"
excerpt: "Safely handle user-supplied code"
date:   2022-01-10 17:31:15 -0700
categories: howto
read_time: true
# header:
#   teaser: "/assets/images/etl.svg"
---

[Puppeteer](https://github.com/puppeteer/puppeteer) is a tool for programmatically interacting with websites.  We use it as a way to provide automation for services that may not provide an API, and we'd like to do it in a way where our puppeteer instance is centralized and easy to manage.  Therefore we've deployed a pod in our environment which hosts a Node.js server running express, accepting puppeteer code supplied by other services and returns the result.

Running arbitrary code is a clear security risk, but in our case it's mitigated by several layers of protection: kubernetes service principles prevent unauthorized pods from submitting requests to this server, and only our team members are able to interact with the service.  Nevertheless, it's a good idea to try to protect the server from malicious (or, more likely in our case, buggy) code.

For this we use a virtual machine *running inside the node server*.  The package allowing this is [vm2](https://www.npmjs.com/package/vm2).

First, import the relevant libraries (using ES6 syntax here):

```javascript
import puppeteer from 'puppeteer'

import {NodeVM} from 'vm2'
```

Now, let's create a puppeteer browser object to send to the VM:

```javascript
var browser = await puppeteer.launch({headless: true,  args: ['--no-sandbox']});
```

The `no-sandbox` argument is needed here because we are using the VM for that purpose.  Next, let's create the VM.  We'll allow it to import only two modules (sleep and axios), and we'll pass an object called `params` as the environment variable.  This allows callers to parameterize their puppeteer scripts.  The VM is given the browser object to work with.

```javascript
var vm = new NodeVM({
  require: {
    external: true,
    modules: ['sleep', 'axios']
  },
  wrapper: 'none',
  env: params,
  sandbox: {browser},
});
```

Let's run the commands!

```javascript
try {
  const vmresult = await vm.run(commands, scriptPath);
} catch(err) {
  // Synchronous errors from the VM are handled here
  processError(err)
} finally {
  // Close the browser object
  browser.close()
}
```

**CAUTION** Since Node is single threaded, the VM runs on that same thread: therefore it is ***not immune*** to intentionally (or otherwise) infinite code, such as `while(1)`
{: .notice--danger}
