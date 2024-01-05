---
layout: post
date: 2024-01-05
place: Minsk, Belarus
title: "Incremental Codebase Contribution"
tags: [ programming, productivity, pdd ]
---

Software development process must be traceable.
How we start from the [requirements](https://www.yegor256.com/2015/11/10/ten-mistakes-in-specs.html)
and how it being converted into a ticket,
then in a [Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)
with a set of commits.
A set.
Not one in the day evening.
Why?
This post answers.

<!--more-->

<img src="/assets/images/2024/01/atlanta-couple.png">

The more reasonable commits you have, the more traceable [git history](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
the project will have.
That's obvious.

But why do ween such "traceable" history of changes?
To trace results right in the middle.
When we have just 1 commit after year/week/day/etc, we don't have a full "trace" of developers' thoughts.
Just 0 and 1 state.
We can't get back and take a look at some unfinished code.
We can't check the _reason_ why this code is being changed.
It's not being developed in just one take.
But our history we will have only a squashed final one.

That's why smart people invent [VCS](https://en.wikipedia.org/wiki/Version_control), including
[SVN (2000)](https://subversion.apache.org/), [Mercurial (2005)](https://www.mercurial-scm.org/)
and [Git (2005)](https://git-scm.com/).

By committing every _reasonable_ change, we are doing a good service for our project.
We can trace every ticket, every requirement till commits we create.

Let's take a look at [l3r8yJ/oop-cop/pull/125](https://github.com/l3r8yJ/oop-cop/pull/125).
It's a pull request we managed to merge just today.
This one solves this [l3r8yJ/oop-cop/issues/85](https://github.com/l3r8yJ/oop-cop/issues/85).

How its being done:
* Project Manager (@zoeself) [assigned me on the task](https://github.com/l3r8yJ/oop-cop/issues/85)
* I've [created a dirty skeleton](https://github.com/l3r8yJ/oop-cop/pull/125/commits/fdd0288b7893f21ef76e9d33d9c97b975678dc50) without unit tests
* Then [added](https://github.com/l3r8yJ/oop-cop/pull/125/commits/0137b8df51f60005843998b3701ac7afbe05b467) a few tests
* Even [more](https://github.com/l3r8yJ/oop-cop/pull/125/commits/6f48b1eb2264ebd5d3ea4b987ce06198bf22bb46) tests
* [Introduced](https://github.com/l3r8yJ/oop-cop/pull/125/commits/627ab1fa727076d0b1600731d109b2d78a3b761a) a [few](https://github.com/l3r8yJ/oop-cop/pull/125/commits/a290d681fa75e91edbe69ccf7f275660ce1060a1) puzzles, and [this one](https://github.com/l3r8yJ/oop-cop/pull/125/commits/f9e4fba1783f7fd1f689955329f0598ea79a1804) gets resolved according [PDD methodology](https://www.yegor256.com/2010/03/04/pdd.html)
* Clean it up, so [Qulice](https://www.qulice.com/) says `BUILD SUCCEEDED`.
* @l3r8yJ approved pull request and @rultor [merged](https://github.com/l3r8yJ/oop-cop/pull/125#issuecomment-1878721917) it into `master` branch

<img src="/assets/images/2024/01/oop-cop-commit-log.png">

This is a traceable development.
Thanks to these [Git commit messages](https://cbea.ms/git-commit/), we can build a thought line.
A reasoning.
This is an incremental contribution.

The result is:
* Each person can take a look at how this ticket being resolved
* Puzzles being converted into new tickets

So, the more traceable your commits, the more maintainable and transparent is the project.
Many incremental commits a day.
That's the way.

Check a few more examples:
[eo-cqrs/eo-kafka/pull/429](https://github.com/eo-cqrs/eo-kafka/pull/429):
* @l3r8yJ reported a [bug](https://github.com/eo-cqrs/eo-kafka/issues/428)
* @l3r8yJ [created](https://github.com/eo-cqrs/eo-kafka/pull/429/commits/f8bf3aa345203c78eea753195efd3a3108fb3933) a [few](https://github.com/eo-cqrs/eo-kafka/pull/429/commits/cea2f00aefa3e2d42082436f3c2d1eff82c6089f) smart commits
* @h1alexbel (me) [asks](https://github.com/eo-cqrs/eo-kafka/pull/429#issuecomment-1685843190) about tests
* @l3r8yJ [introduced](https://github.com/eo-cqrs/eo-kafka/pull/429/commits/fe52c78899a85dd587f0ffdd913140d4fc4ec591) new puzzles, in order to [cut corners](https://www.yegor256.com/2015/01/15/how-to-cut-corners.html)
* @h1alexbel (me) [checked again](https://github.com/eo-cqrs/eo-kafka/pull/429#pullrequestreview-1588325491), make a comment
* @l3r8yJ [responds](https://github.com/eo-cqrs/eo-kafka/pull/429#discussion_r1300937458) me back, we merged

[objectionary/eo/pull/2731](https://github.com/objectionary/eo/pull/2731):
* I found an old [issue](https://github.com/objectionary/eo/issues/2422)
* Created a few commits including [skeleton](https://github.com/objectionary/eo/pull/2731/commits/1bedabd87023b1b0d4eb31f2d693b80de2e65913), [adding/removing](https://github.com/objectionary/eo/pull/2731/commits/bd4dbaec0010e885940ee70fc88bec1003b51b44) object from `Place.java`
* [Reported](https://github.com/objectionary/eo/pull/2731/commits/84fb590e0e142987329aea05f669ef0e1216ac94) a broken test with `@Disabled` + puzzle, as explained [here](https://www.yegor256.com/2023/07/25/contribute-disabled-tests.html)
* @maxonfjvipon [asked](https://github.com/objectionary/eo/pull/2731#pullrequestreview-1796502306) me to resolve code duplication inside `Place.java` that task also [mentioned](https://github.com/objectionary/eo/issues/2422)
* I [fixed](https://github.com/objectionary/eo/pull/2731/commits/25c022263132d9917ed8ad5349ae82ba38c9f05d) that, requested a re-review
* @maxonfjvipon approved
* @yegor256 (an Architect) approved too and @rultor merged
