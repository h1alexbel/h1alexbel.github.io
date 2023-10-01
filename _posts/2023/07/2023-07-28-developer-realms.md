---
layout: post
date: 2023-07-28
place: Minsk, Belarus
title: "Developer Realms"
tags: [process, career]
---

How did you deploy software to production?
Did you use Kubernetes?
No, I'm not a DevOps guy, I'm a Developer!
OK, which documents you produce and activities in which you participate
as a software developer besides coding?
I'm just writing code, I'm a Developer.
I'm here a lot these days.
And it's a very wrong attitude.
Here, in this blog post, I'm saying what you
need to know and regularly practice as a software developer 
to become a ~~CTO~~ self-capable unit of work.

<!--more-->

<img src="/assets/images/2023/07/trio.png">

I strongly believe that being a developer, besides coding, you need to know
how to help your project.
Remember, when coders create lines of code,
professional developers create software that works.
<br>
What are the steps to build from zero some more or less
stable software product?

According to [**RUP**](https://en.wikipedia.org/wiki/Rational_unified_process), there are 9 disciplines
in a common software project:
* Business Modeling
* Requirements
* Analysis & Design
* Implementation
* Testing
* Deployment
* Configuration & Change Management
* Project Management
* Environment

All of them you need to be aware of!

And don't get me wrong.
It's not about being Jack of all Trades.
You need to be a niche specialist, a niche-focused **expert**.
Imagine X & Y coords, on X is your disciplines,
while on Y is your tech stack you're expert in.
**If you are a niche-focused expert,
and at the same time, know all activities in software project,
you will the MVP on each project**.

I've summarized all the RUP phases into 
the following disciplines, the developer realms:

* Business Modeling
* Requirements
* Design
* Implementation
* Testing
* Deployment
* Configuration
* Project Management

## Business Modeling

First things first.
Business modeling.
Here you need to have knowledge of use-cases modeling
and all things related to inception phase.
Try to follow simple rule: docs over talks.

## Requirements

Then, after business modeling, we need to 
start collecting requirements.
To do it right, you need to know what [**SRS**](https://en.wikipedia.org/wiki/Software_requirements_specification) is.
Moreover, you need to be aware of [**rules and 
best practices**](https://www.yegor256.com/2015/11/10/ten-mistakes-in-specs.html) when it comes to requirements.
And again, start preparing the right documents.
I'm usually creating one long README.md file and 
then commit it into git repo.
So, your tooling can be: Git + Markdown/LaTeX.

## Design

To become an [**architect**](https://h1alexbel.github.io/2023/06/11/arc-tools.html), you definitely need 
to become very successful here.
First, master your UML skills.
You must be able to draw simple UML sketch,
that will be understood by all stakeholders, not only by your developers.
So, simplicity is also your instrument.
I believe that the simpler you present and sketch,
the better architect you are.

You need to know how to formulate design not only in diagrams,
but also in text.
Learn SAD by RUP or SDD by IEEE.
I suggest you to pick one standard and integrate it in your README.
Again, produce documents that emphasize all your decisions.

## Implementation

Finally, coding!
Your main focus should be on [**Software Maintainability**](https://h1alexbel.github.io/2023/06/25/maintainability-first-others-second.html).
It's lots to learn how to reach it.
It's hard.
Your open source skills can boost it significantly.
You can gain this skill only by creating and maintaining
your own software packages and products.
Simply put, code reusability is the main virtue here.
While duplication is a sin.

## Testing

First, understand why we need testing.
It's not about comparing actual and expected results.
It's about finding bugs in your software.

As software developers,
we need to know how to create maintainable tests.
Here, maintainability reflects on your design.
If you have a lot of procedural stuff in your production code,
you will notice that it's so hard to maintain tests for such code.
I even believe that to become a better developer, you need to focus
mainly on automated tests and test architecture.
Some tools that will help you here: unit, integration, e2e, load,
performance testing.
Code coverage and any test control are also applicable here.

## Deployment

My favorite here.
You need to be well-versed how your software gets 
deployed to the end users.
No excuses.
Popular tooling is some cloud provider like [**AWS**](https://aws.amazon.com/) or similar,
[**Kubernetes**](https://kubernetes.io/), and [**Docker**](https://www.docker.com/).
And your deployment must be configured at
the start of the project
and be fully automated like [**here**](https://h1alexbel.github.io/2023/06/02/deploying-to-gcp.html).

## Configuration

You need to know how to configure your CI/CD pipelines,
how to set up VCS.

Here, you can also merge [**Deployment activities**](#deployment) and configuration
management by using [**IaC**](https://en.wikipedia.org/wiki/Infrastructure_as_code).
[**Terraform**](https://www.terraform.io) is a good one.

## Project Management

To grow, you need to understand project management.
I'm not talking about Agile fairy tails, I'm talking about
some serious project management rules and best practices.
And you can't learn them in one day.
From myself, I suggest to read [**PMBOK**](https://www.pmi.org/pmbok-guide-standards/foundational/pmbok), [CMMI](https://cmmiinstitute.com/) and try to apply them
into your project workflow.

Let me summarize all the stuff: you need to go full cycle.
The best way to do it is to create your own [**pet**](https://h1alexbel.github.io/pets.html) project.
There, you will practice all the disciplines, 
and you will be able to do them in a large-scale project.
