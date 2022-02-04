---
layout: single
title: The *real* State of the Art
permalink: /legal-tech/sota/
toc: true
read_time: true
sidebar:
  nav: "lt-pages"
---

<!-- # State of the Art -->

## Keeping up with the Field

Reading the headlines in the AI/ML space, the breakneck pace of technological advances is immediately clear. Increasing compute resources combined with clever model partitioning and architectures mean that just about every week, another unprecedented result is being reported. It’s easy to feel overwhelmed, trying to keep up with the latest in what is almost literally an exploding field.

Fortunately for my team, most of the problems we’re working on can be more than adequately addressed by what was the state of the art *several years back.*  This is owing to the nature of the problems themselves: often they can be written in a way that makes them amenable to tried & true solutions.  This is just as well; in legal we are often limited by a shallow pool of data, leading us to draw on off-the-shelf or “diet-data” technology.

## Diet-data in Practice

A clear example of this is our use of word embeddings.  Word2Vec debuted in 2013, upending the world of language processing: perhaps the most profound discovery since that of the Rosetta Stone in 1799.  Representing words as vectors in a space of meaning, this has unlocked all manner of NLP advances: sentiment classification, text generation, remarkably good machine translation.  Today’s latest tech (think Google’s BERT or OpenAI’s GPT-3) all exploit word vectors to deliver their astonishingly (and dangerously) good results.  However, at their core, it’s the word2vec innovation that underpins their success.

We imported that specific idea for use in our firm’s products; we don’t (yet) train our own models, since the vectors available freely from the open source community far surpass the quality of any we could train from our small datasets.  Our products work remarkably well; not because we run huge models with many layers, but because we use the foundational word2vec technology.

## Exploiting Structured Data

Another, even more basic example is that of page classification.  Image classifiers were the original use case for CNN’s, and continue to be an area of hot research: GANS producing “Van Gogh” paintings rely on them to discern fact from fiction, and virtual personal “shoppers” use them to comb online retails for items matching your taste.

For our use cases, where documents are regularly structured, we discovered that sophisticated (and sometimes finicky) CNN’s are overmatched to our tasks at hand: a combination of deliberate image rasterization and cutting hyperplanes deliver exceptional performance, for a fraction of the complexity.

## Delivering Effective AI

So, while AI research labs will continue demolishing seemingly unsolvable problems at an astonishing rate,  the situation where the rubber hits the road is rather different.  Data professionals at the leading edge of their company's needs are best served recognize when to invest the time and resources in complex, cutting-edge techniques, and when to deploy battle tested, well-understood tools that have been proven to deliver.