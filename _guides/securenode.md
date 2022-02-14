---
layout: single
title:  "Secure, headless Chromium in a Docker container"
date:   2021-12-15 11:00:15 -0700
categories: howto
read_time: true
---

## Headless Chromium

One of the services we have in our Kubernetes deployment runs a Node process to instantiate [Puppeteer](https://github.com/puppeteer/puppeteer), allowing automatic interfacing with websites.  The Puppeteer library manipulates Chromium, so we needed a way run Chromium -- in a headless fashion -- inside of a Docker container.

This turned out to require some experimentation.  Starting with a lightweight Alpine image, we first installed the Chromium package.  Unfortunately, while this is enough to launch Chromium off the command line, the browser won't actually be able to render any pages.  For that, we need a number of other packages:

```Dockerfile
FROM alpine:3.15

# Installs latest Chromium package.
RUN apk add --no-cache \
      chromium \
      nss \
      ca-certificates \
      freetype \
      freetype-dev \
      harfbuzz \
      ttf-freefont
```

`nss` and the certificates provide network security, while the other packages allow Chromium to render the text presented in web pages.  It's possible that a base Ubuntu 20.04 may not require these additional packages, but they are needed for headless Chromium on a lighter-weight Linux container.

## Securing the environment

The container runs a node.js server to manage puppeteer, and we'd like to this to i) not run as the root user and ii) be able to listen on port 80.  Conventionally, ports below 1024 are accessible only to processes run by the root user, so we must allow a non-root process to bind to a priority port.  Setting the capabilities for this happens via `setcap`, and the following line allows the node process in particular to bind:

```Dockerfile
RUN setcap cap_net_bind_service=+ep `readlink -f \`which node\``
```

The last argument to setcap simply identifies the path of the process called `node`
{: .notice--info :}

Next, we want to run our process as a non-root user, to prevent container breakout.  Let's add a user and corresponding group, switch to that group, and switch to the working directory:

```Dockerfile
# Prevent container breakout
RUN addgroup nodejs && adduser -S -G nodejs nodejs

USER nodejs

WORKDIR /app
```

## Dumb-init

In a conventional Docker container, any command executed by `CMD` will be run with PID 0.  PID 0 is special to the Linux kernel in a number of ways, one being that it's immune to SIGTERM.  It turns out that subprocesses spawned by the PID 0 are *also* immune to SIGTERM, so any Chromium instances launched by Puppeteer running as PID 0 *will never close* -- even if Puppeteer directs the browser to end its process.  This can quickly lead to resource pressure and eventually a crashed container.

The solution is [dumb-init](https://github.com/Yelp/dumb-init), a kind of proxy process which runs as process 1, and launches the command in question.

```Dockerfile
ENTRYPOINT ["/usr/bin/dumb-init", "--"]

CMD ["node", "."]
```

**Tip** To use `setcap` and `dumb-init`, the binaries must be added to the environment: `RUN apk add --no-cache libcap dumb-init`
{: .notice--info :}