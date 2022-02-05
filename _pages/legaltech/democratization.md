---
layout: single
title: Democratization of AI
permalink: /legal-tech/democratization/
toc: true
read_time: true
sidebar:
  nav: "lt-pages"
---


The debut of transformers in 2018 poured gasoline on the fire of natural language processing -- already showing success on some tasks after the development of word embeddings, transformer models significantly upped the bar on the capability of the technology.  Sentiment analysis, language classification, translation, and even text generation are becoming routine problems for NLP professionals.  Today's most advanced models, such as OpenAI's GPT-3 and NVIDIA's Megatron, contain hundreds of billions of parameters -- and can "engage" with a user in ways that feel almost human.

For a small group like ours, the idea of designing -- much less training -- a model like Megatron is a non-starter.  Yet would still hugely benefit from the technology; law is driven by language, so NLP tasks abound in our industry.  Fortunately for us, a wave of "AI democratization" has allowed groups like ours to quickly -- and effectively -- deploy some of the latest technologies to our users.  This has been made possible by two developments: the release of pre-trained models and the construction of user-friendly API's.

<figure>
  <img src="{{ '/assets/images/sota.jpg' | relative_url }}" alt="SOTA" class="full">
</figure>

## Pretrained Models

Training an advanced transformer model requires billions of pieces of training data, and access to extremely powerful compute -- on the scale of a datacenter.  This is out of reach for all but the largest Silicon Valley firms, who recognize that the **applications** of those models are driven as much by large players as by small business.  As a result, model specifications and parameter values **after training** have been made available to anyone interested.

There is an exception to this -- given the capability of OpenAI's GPT-3 to generate real-sounding language, access to it was restricted until OpenAI was confident it could detect misuse.
{: .notice--danger}

Repositories like huggingface and Nvidia NGC now host models trained on a wide variety of tasks, ready to use at the click of a button.  Groups like ours can download and deploy a model in literally a matter of minutes.

## Friendly API

Model specification and parameter values are useful, but require a developer to write potentially complex wrapping code to deploy the model.  The development of user friendly API's, like `sentence-transformers`, a subpackage of `transformers` from huggingface, allow a user to instantiate and infer from a model in literally fewer than ten lines of code.  While a good understanding of the model and its behavior is fundamental to getting good results, the "sausage making" required to get things off the ground has been largely eliminated.

## Silicon Valley AI in Rocky Mountain Law

These developments have allowed us to bring near-SotA technology to bear on the problems around our firm: we have deployed semantic search tools, text suggesters, language classifiers, even some text generation.  None of this would have been possible without access to pre-trained models over simple API's.



