---
layout: post
date: 2023-01-21
place: Warsaw, Poland
title: "Maintainable Project Template"
tags: arc maintainability
---

Nowadays, we live in the era of the open source software. So, we are judged by the projects that we're doing here.
But how to maintain high quality not only in the projects on your [GitHub](https://github.com) account?
Most enterprise software comes with big mess.
Successful [software architect](https://www.yegor256.com/2014/10/12/who-is-software-architect.html) creates maintainable software.
In this blog post I will share my key points of successful project, and how to build it from the 1st commit.

<!--more-->

<img src="/images/2023/01/bsc.png">

# ```Build pipeline```

Build pipeline is absolutely mandatory. And this is [first](https://www.youtube.com/watch?v=TcD6jJKaLcg&t=78s), what you should configure when you're starting
new
project.
I recommend [GitHub actions](https://github.com/features/actions), but if you aren't at GitHub, you can use [Travis](https://www.travis-ci.com) or [CircleCI](https://circleci.com). They are also
pretty well.
Here is an example of maven build using GitHub actions.

```yaml
name: mvn
on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]
jobs:
  build:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v3
      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'
          cache: maven
      - name: Build with Maven
        run: |
          mvn clean install
```

# ```Merging bot```

All pull requests should be merged not by a [human](https://www.yegor256.com/2014/07/21/read-only-master-branch.html),
but by bot. On GitHub, you can find lots of them. I use and recommend
[Rultor](https://github.com/rultor).

[More details](https://www.yegor256.com/2014/07/24/rultor-automated-merging.html)

# ```README```

Proper README styling really helps in project understanding. Unfortunately, most people don't style their README,
instead, they go to Confluence/Wiki pages and post unlimited amount of documents and information about the project. 1
README file must be your documentation. Period.

There are few points that definitely should be in your README file:

- Useful badges (CI status, deployment status, code coverage report, license, etc.)
- What it is and what problems does it solve
- Prerequisites
- Quick Start
- How to contribute

# ```Tickets```

Your project should be traceable. In order to achieve that you will need to discipline your team. Just follow simple
rule: don't make changes without a ticket. Tickets should be the smallest as possible. If it doesn't - decompose it into
smaller ones. I even suggest you to
try [Puzzle-Driven-Development](https://www.yegor256.com/2017/04/05/pdd-in-action.html) and micro-tasking.

# ```Dependency management bot```

After your project will be successfully deployed and used by many users you should watch your dependency list. Time to
time check if some dependencies are too old or not. Don't do it manually, instead delegate this work to the
bots. [Renovate bot](https://github.com/renovatebot/renovate) can be a good fit.