---
layout: post
date: 2023-01-21
place: Minsk, Belarus
title: "Maintainable Project Template"
tags: arc maintainability
---

Nowadays, we live in the era of the open source software. So, we are judged by the projects that we're doing here.
But how to maintain high quality not only in the projects on your [GitHub](https://github.com) account?
Most enterprise software comes with big mess.
Successful [software architect]() creates maintainable software.
In this blog post I will share my key points of successful project, and how to build it from the 1st commit.

<!--more-->

<img src="/images/2023/01/bcs.png" width="1200" height="650">

# 1. Build pipeline

Build pipeline is absolutely mandatory. And this is [first](yegorlink), what you should configure when you're starting
new
project.
I recommend [GitHub actions](), but if you aren't at GitHub, you can use [Travis]() or [CircleCI](). They are also
pretty well.
Here is an example of [maven]() build using GitHub actions.

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

# 2. Merging bot

