---
layout: single
title: Secure Training Data
permalink: /legal-tech/secure/
toc: true
read_time: true
sidebar:
  nav: "lt-pages"
---

### Text Brokers

Law firms principally generate and consume text -- not only is this reflected in their final legal products, but it's also true in the countless intermediate steps involved in crafting those products.  As providers of automation solutions in the process, we are therefore also primarily concerned with language, which means we're also concerned with protecting it.

<figure>
  <img src="{{ '/assets/images/letters.jpg' | relative_url }}" alt="Text Brokers" class="full">
</figure>

### Trading in Language

Language--and the annotations we may want to give it--is naturally unstructured, so we use NoSQL databases to store text for a variety of purposes: document generation, language suggestion, etc.  Since some clients have stringent data storage policies, simply limiting access to a plain-text database may not be enough to satisfy security requirements.  How, then, should be we guarantee data security without compromising performance?

The answer depends, in part, on how we put the data to use.  In cases where the data is stored for  batch-processing, we may elect to encrypt the entire record on write and decrypt on read.  This obfuscates the records entirely and reflects the highest level of security (assuming proper key management and database-interface architecture).

### Best of Both Worlds

However, in many cases we wish to be able to query the data: for example, returning a record from a certain time, by a certain author, etc.  Today's NoSQL architectures make such queries extremely fast, but only if the data isn't encrypted.  One solution to this problem in the tabular world is to create an extra "hash-column," which stores an obfuscated form of the value.  The querying service then calculates the hash and queries for it, allowing equality queries to happen in the encrypted space.

One could hash or encrypt the values of NoSQL record schemas and follow the same approach, but this severely hamstrings the querying power: one cannot query for `any date after KAGU/IDENF==`.  The approach we've taken is to partition our records into encrypted and plaintext parts.  Data we wish to query on, and cannot be subject to security requirements, rests in plaintext in the database.  Other keys are encrypted, so that the we can still benefit from the query infrastructure without compromising data integrity.

### Fundamental Balance

This discussion, though specific to data storage, reveals a tension acutely felt inside of a legal organization: that between being careful and yet still building tools that are functional.  Law firms have robust loss-prevention departments, with stringent rules regarding data protection and retention.  As the caretaker of the data processed through our systems, this is not an area in which one wants to cut corners.  No law firm--or data professional--wishes to end up in the news for the wrong reasons.