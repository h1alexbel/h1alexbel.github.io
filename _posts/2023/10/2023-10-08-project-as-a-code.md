---
layout: post
date: 2023-10-08
place: Minsk, Belarus
title: "Project as a Code"
tags: [ process, pm ]
---

We're living in an [Agile](https://www.yegor256.com/2016/07/11/mistakes-in-agile-manifesto.html) world.
Customers trust architects, programmers, designers and other product makers to deliver new software
to the market.
What is the result of this trust?
In the best cases, software doesn't meet the requirements,
the product is ~~very hard to trace~~ untraceable,
and onboarding takes weeks.
In most cases, customer [has nothing](https://www.yegor256.com/2017/11/21/trust-pay-lose.html) out of it.
Enough is enough.
Introducing Project as a Code, the software development methodology dealing with
project traceability, artifact management and clearness to the customer on what's going on.

<!--more-->

## All Docs are under Control

The main problem is that we're trying to solve is that most projects
are untraceable.

* "Why is this decision made?"
* "How to check out the code before new feature requests are employed?"

In 2023, most software teams manage quality gates, a.k.a CI/CD pipelines
in their code.
Why can't we apply it to the documents we have?
We do.
Each change is controlled since kept in git and has to be pass
a merge pipeline before getting into `master` branch.

Thus, all the software documents including [requirements document](),
Use-Cases, diagrams, and others can be defined in the source code
and empowered with control and integrity checks,
or even with AI post-analysis tools. 

## Manage Artifacts, not People

## Communication is Problem-Oriented

## No Backlog, Puzzles instead
