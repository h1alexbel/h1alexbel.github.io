---
layout: post
date: 2023-04-23
place: Minsk, Belarus
title: "Worst & Best practices of Software Development"
tags: maintainability quality
---

Most projects now failing not because of stupid or lazy developers, but because of low quality-level of the software
projects.
Low quality of CI/CD pipeline, Code Reviewing, weak or no Testing, no Style Checking, poor Documentation and so on.
This blog post one by one will explain what should be done, so you can call your project of high quality and contribute
to it with no fear.

<!--more-->

<img src="/images/2023/04/broen-dinner.png">

As software development has evolved, it has become clear that the main reason for project failures is not necessarily
due to the incompetence of developers, but rather the low quality-level of the software projects themselves.
Gone are the days when blaming lazy or stupid developers was the default explanation for project setbacks.
Instead, the focus has shifted to identifying the root causes of low quality in various aspects of software development,
such as the CI/CD pipeline, code reviewing, testing, style checking, and more.

Let's start from the deadly sins that affect quality and can ruin the
entire [project](https://h1alexbel.github.io/2023/01/21/maintainable-project-template.html).

## §1 No Architect is assigned

If no such a person, who is responsible for success and failures of software project, quality is not being ensured then.
Architect is mandatory for ensuring quality and making technical decisions related to project.

## §2 Compromises

Compromise may arise in the form of technical debt, which refers to the practice of postponing necessary tasks,
such as refactoring or addressing architectural flaws, in favor of meeting immediate goals or deadlines.
Technical debt can accumulate over time and impact the maintainability and stability of the software,
leading to increased costs and challenges in the future.
This could result in incomplete testing, inadequate quality checks,
or insufficient documentation, leading to lower quality software.

## §3 No Code Reviews

Code reviews is absolutely mandatory tool.
This activity is necessary to reject your code.

## §4 No traceability

Each Pull Request has to be linked to ticket.
Don't change your code until related ticket is submitted.
That's it for traceability.
Project should be traceable in order to be maintainable.

## §5 No Automated Tests or weak one

Automated tests is your mandatory tool for being fast and productive developer.
I don't want to hear your excuses like no time, the boss doesn't like it, or others.
You just don't know how to write tests.
Fix it, and you will be happy.

## §6 Code Coverage is not collected

How much of the production code you actually touch?
The more, the better is your testing.
But it is not a guarantee that your tests are good.
But if your coverage is low then this is more or less definitely a guarantee that your testing is weak.
Also, Code Coverage must be part of your [build](#7-build-is-not-automated) pipeline.

## §7 Build is not automated

Your code should be built into one executable artifact.
Don't do it manually, create a script or use some tools like `Maven`, or `npm` to build your code.
Period.

## §8 No strict style checking

Just look at [this](https://github.com/is/jsch/blob/master/src/main/java/com/jcraft/jsch/ChannelSession.java#L234-L263).
Stuff like this will demotivate developers to contribute to your project.
Styling should be controlled as you want project be stable and maintainable enough.
The best tools for that is linters and static analysis.

## §9 Deployment is not automated

Deployment automation is a crucial aspect of modern software development practices
that can significantly impact the quality and reliability of software projects.
Manual deployment processes can introduce challenges such as human errors, inconsistency,
and inefficiencies, while automated deployment can offer benefits such as repeatability, consistency,
and improved CI/CD practices.

## §10 No simple and single page of documentation

No WIKIs please.
Documentation should be placed in one [**elegant**](https://www.yegor256.com/2019/04/23/elegant-readme.html) README, and should be in Git history.

How to fix it?

## 1. Master branch is ready only

Only pull requests from feature branches to the `master` one.
Read [this](https://www.yegor256.com/2014/07/21/read-only-master-branch.html) for more details.

## 2. Documentation in simple README file

As we discuss in [§10](#10-no-simple-and-single-page-of-documentation),
your documentation should be in [one](https://www.youtube.com/watch?v=Qxvk9z0tEP8) file called README.
Quite simple rule, I believe.

## 3. Build quality wall

Build a quality wall that will automatically reject the code,
that don't fit to us according our quality standards.
What you should include in your wall:

- Code Compilation
- Execution of Automated Tests(Unit and Integration)
- Linters
- Static analyzers
- Test coverage checks
- ... everything you consider to do!
  Remember: the stricter, the better.

## 4. Don't ask them to write high-quality code

Once the [wall](#3-build-quality-wall) is built we don't need to say to developers:
"Please, write only a good code, I'm counting on you".
No sense since quality wall is configured to reject bad code.
Instead, say: "Please, write code faster"

## 5. Independent Quality Reviews

Independent Quality Reviews is indicator of ensured the quality of software projects.
IQRs involve an objective and impartial evaluation of the software development process, deliverables,
and outcomes by an independent team or individual who is not directly involved in the development process.
These reviews provide an unbiased assessment of the quality of the software project
and help identify potential issues and areas for improvement.
