---
layout: single
title: Finding Language
permalink: /legal-tech/etl/
toc: true
read_time: true
sidebar:
  nav: "lt-pages"
---

Textual data is the lifeblood of a lawfirm, but gathering and processing it can be a challenge.  Traditional "plug & play" ETL pipelines, which may be well-suited to tasks like image harvesting, may struggle with identifying specific types of paragraphs from particular types of documents.  As a result, the data engineer must be willing to get hands dirty, building their own ETL toolchain.

<figure>
  <img src="{{ '/assets/images/shovel.jpg' | relative_url }}" alt="ETL" class="full">
</figure>

Firms maintain vast amounts of information in their document management system (DMS).  This system often contains minimal structure, as users organize their corner of the filesystem in the manner that makes most sense to them (reflecting the distribtued way the firm itself is managed).  For a data engineer tasked with finding all of the objection paragraphs within a certain class of documents (e.g., all RFPs), the unstructured nature of both the language and the filesystem can make for an exciting adventure.

<figure>
  <img src="{{ '/assets/images/etl.svg' | relative_url }}" alt="ETL" class="full">
  <figcaption>A fully automated ETL pipeline is built up over many cycles of iteration and discovery, starting with user-executed scripts.</figcaption>
</figure>

## Archaeological Excavation

The first step for our team is an interview with the business stakeholders requesting the new functionality.  They're usually interested in gathering a certain class of language, and will show us how they've done it "up to now:" visit the DMS, query it for some keywords, then try to intuit which of the hundreds of results might be the most useful.  Most DMS search tools are rudimentary at best, and amount more to keyword filters than true semantic search.

Using that same approach, we'll start by trying to source documents in the same way.  We dig into the DMS, trying to get a sense of where the documents are buried, and if a programmatic approach to processing the filestructure is more appropriate, or if searching the DMS and then culling the results will be most useful.  We do all of this in user sessions, usually iPython shells, building up knowledge of where the data lies in what is often highly uneven terrain.

## Surveying the Site

Once we have a sense of where the documents live, we can also open them up and take a look at them.  Though they may all be from one "class" (e.g., RFPs), that usually glosses over the considerable differences among them.  Different authors prefer different styling, both in the their language and in their document layouts.  Automatic processing, therefore, requires a comprehensive survey of the different types of language that a tool may encounter.  We aim to gather 90 - 95% of the relevant passages from the sourced documents, recognizing that we will almost always encounter outlier language that requires manual processing.

## Taking Inventory

Determining how best to store the records we recover can really only be done *after* we have a sense of the scale.  After excavating the DMS and then surveying the records we find, we are finally in a position to assess which database storage option will be most appropriate.  For wildly varying text records we opt for NoSQL solutions, while relational tables will make sense for highly structured and predictable results.

## Automatic ETL

The initial phases of ETL all happen in a highly interactive manner: based on what we find, we update and adjust our toolchain.  Over time, as we step through the processes of archeaology and survey, we are able to shape and build confidence in an automatic ETL solution.  In many cases, we're able to recycle pieces of our pipelines (DMS integrations, document loading and processing) across different use cases, but the key questions of *where, in what form, and how much* almost always require direct human involvement.