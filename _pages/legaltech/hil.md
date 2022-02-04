---
layout: single
title: Humans in the Loop
permalink: /legal-tech/hil/
toc: true
read_time: true
sidebar:
  nav: "lt-pages"
---

<!-- ## Humans in the Loop -->

<center><b>
When you have no data, how can you introduce automation?
</b></center>
{: .notice--danger}

### Starting from Scratch

A perennial problem in data science is *acquiring good quality training data.*  Particularly in our area - law - there is very little of it.  In fact, some of our custom models start with literally zero elements of training data.  How can we build models from scratch, without spending large amounts of time building a data library?

<figure>
  <img src="{{ '/assets/images/keyboard.jpg' | relative_url }}" alt="Construction" class="full">
</figure>

### Learning Opportunities

Our approach puts that emphasis squarely on the users of our technology: they not only use the tools, they teach them.  We treat each interaction with our technology as a learning opportunity.  When a new model is developed, it starts with little or no predictive power.  Recognizing its own limitations, the system therefore makes no suggestions or decisions.  However, as users interact with it, they show it how they'd like it to behave in the scenarios they present it with.  Cataloging these interactions builds up a corpus of training data, so that over time (relatively quickly, in practice), the system gains predictive power and can begin offering a level of automation.

**This differs from traditional, "mechanical turk" human-in-the-loop approaches in one key way:**<br><br> The humans are not dedicated data janitors, but users actually deriving value from the tool while they improve the quality of the data used to train it.  Specifically, they are in-band consumers of the tool they're helping to improve.
{: .notice--info}

### Novel System Architecture

Supporting this paradigm required us to intentionally design our system around the learning opportunities: the architecture is constantly on the lookout for interaction points between users and the tools they're engaging with, and is quick to fold a new piece of training data into the corpus and update the models.  

Critically, model updates don't happen if performance declines.  Users sometimes introduce noise into the data set, commonly by using the tool in a way unlike the normal usage patterns.  This is owing either to

- a mistake or 
- a genuinely novel use case.

In response to the former, the system identifies the noisy data and underweights or removes it; in response to the second, a new model or response class may be defined to capture the legitimate, but different, use case.

For us, the ability to leverage the insights and expertise of our users, while they benefit from our tools, is a uniquely powerful position from which to train and deploy automation technology.