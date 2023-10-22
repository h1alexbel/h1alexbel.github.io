---
layout: post
date: 2023-10-22
place: Minsk, Belarus
title: "Project as a Code"
tags: [ process ]
---

We're living in an [Agile](https://en.wikipedia.org/wiki/Agile_software_development) world.
Customers trust architects, programmers, designers and other product makers to deliver new software
to the market.
What is the result of this trust?
In the best cases, software doesn't meet the requirements,
the product is _untraceable_, onboarding [takes](https://www.youtube.com/watch?v=WwjU1ORdz4s) weeks,
and all the members are _demotivated_ and hate their job.
In most cases, customer [has nothing](https://www.yegor256.com/2017/11/21/trust-pay-lose.html) out of it.
Enough is enough.
Introducing Project as a Code, the software development philosophy that deals with
project traceability, artifact management and clearness to the customer on what's going on.

<!--more-->

<img src="/assets/images/2023/10/forbrydelsen-board.png">

## Traceability and Clearness

The main problem is that we're trying to solve is that most projects
are untraceable and unclear enough for their crew.

* "Why is this decision made?"
* "Why I get less than John doing the same Java coding?"
* "How to check out the code before new feature requests are employed?"

We are here for clearness and traceability on your project.

## Documents Under Control

In 2023, most software teams manage quality gates, a.k.a CI/CD pipelines
in their code.
Why can't we apply it to the documents we have?
Project as a Code means that.
Each change is controlled since kept in [git](https://en.wikipedia.org/wiki/Git) and has to be pass
a merge pipeline before getting into `master` branch.

Thus, all the software documents including
[requirements document](https://www.yegor256.com/2015/11/10/ten-mistakes-in-specs.html),
Use-Cases, diagrams, and others can be defined in the source code
and empowered with control and integrity checks,
or even with AI post-analysis tools.

Tickets already [can](#no-backlog-puzzles-instead) be defined in the code,
why documents can't be?

Here are a few ideas of how it can be defined:

The project definition:
```yaml
project:
  name: ai-secret
  description: AI Secret Project
  budget: 1mil$
  private: true
  date:
    start: 2023-10-03
    end: 2023-11-30
team:
  - email: aliaksei.bialiauski@hey.com
    roles: ARC
  - email: john@google.com
  - roles: DEV
  - email: mary@gmail.com
  - roles: [PM, PO]
stakeholders: ...
```

Use-Case definition example:
```yaml
name: where User can identify himself
rdql: |
  The user logins with his credentials, if he has them.
  Otherwise, he registers in the system first.
```

## Results are Traceable

Since we have all the stuff in the source code,
we can check, integrate and validate artifacts and results.
Thus, _productive_ work can end only with lines of code, or new documents
but not with new Slack messages or meetings.

Also, a project means a role-based project.
We don't know how to make customers happy.
Developers know how to develop Java classes;
PMs know how to manage a project.

Since this, a properly managed project's required not just people, or just programmers.
We need concrete individuals capable in their realms.
The Goal is to find the best and segregate them using simple project roles.

## Happy or Quit

All the team members know everything about the project.
They know how to boost their salary, what kind of performance
project expects from them, what are the project risks, and so.
They know what are the rewards.
They know how they will suffer if a task is not completed or deadline missed.
Simply put, they are not slaves kept in the dark.

On the project you are happy and productive, or you _quit_.

## No Backlog, Puzzles instead

Project as a Code inherits [Puzzle Driven Development](https://www.yegor256.com/2010/03/04/pdd.html).
All the bugs kept as puzzles.
Puzzles are defined in source code and discovered
_while working_ on tasks.
Tasks mean new tasks down the road.
But instead of keeping all of them in our minds,
we are simply doing it in the code.

Thus, we are slightly evicting
a PM role from the definition of done.
Now, we are formulating by us, people who know the reality.

## Ecosystem

Moreover, this philosophy can be employed with
a new software development platform for enabling that kind of
productivity for all team members and stakeholders on a project.

We can go even further and make the platform programming language native.
I don't think it can be Java/Kotlin or TypeScript.
Simply because those langs are good in enterprise/product development,
but we need some sort of DSL, like Ruby and Groovy, or
just plain XML/YAML sheets.
