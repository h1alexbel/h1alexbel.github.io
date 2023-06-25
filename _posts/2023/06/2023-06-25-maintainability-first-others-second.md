---
layout: post
date: 2023-06-25
place: Minsk, Belarus
title: "Maintainability first, Others are secondary"
tags: maintainability arc
---

I don't even know where to start.
Maintainability is the main Non-Functional Requirement (NFR) in software development to aim for.
For most software projects, maintainability is the most important thing.
I believe this post answers on why.

<!--more-->

<img src="/images/2023/06/broen-gang.png">

I want to start a talk about maintainability with great Martin Fowler quote:

> _Any fool can write code that a computer can understand.
Good programmers write code that humans can understand._

So, right now an average developer is more expensive than any high-performance server.
20 years ago it was so different, servers, CPUs, GPUs, RAM and everything else
from a hardware world was so pricy compared to people who working with that.
Now, thanks to **[AWS](https://aws.amazon.com)**, you can rent any server you want,
and it will cost 100x less than it was 20 years ago.
Many years ago when computers were expensive, developers needed to be algorithmic, science people.
Now, most developers don't even know where their Python code is being executed.
We start don't care about servers, algorithms, and performance execution,
again in most software projects.
**But we still care not enough about maintainability**.

I believe it's time to change our mind, our thinking about developing the software we develop.
We need to focus more on maintainability,
we need to start looking at it as **the primary Non-Functional Requirement we have**.
Why?
Well, because, high maintainability helps to a random stranger to fix the bug.
You can say you don't have those strangers.
Then imagine you working on the project and a new developer is on-boarding.
New developer will ask a trillion of questions if
the code or documentation is not clear enough.
Each question (I treat them as new bugs) is potentially
improving the maintainability of the project.

<span style="color:red">WRONG</span>.

`How this java class works?` - John.
<br>
`*explains actually how it works*` - Hanna.

<span style="color:green">RIGHT</span>.

`How this java class works?` - John.
<br>
`Class along with docs are refactored, try again` - Hanna.

> _Making your code open and encouraging programmers to report bugs when something
is not only broken but unclear—are the best two ways to achieve high maintainability.
-Yegor Bugayenko_

Make a rule for a project you're working:
if you have questions and/or you don't understand how things working report a new bug.

The more questions you will have,
the more chances you have to improve the maintainability of your project.
In the end, your code will be super maintainable.
As a result, your team will be faster and more automated, and ~~you can get rid of meetings~~.

The best developers use instruments to keep code in a good state:
1. Automated Tests including Code Coverage
2. Static Analysis, Linters

These tools on each build verify the quality level of software you develop.
But those tools are not fix the problems with maintainability
if we treat maintainability as an ability to understand and
fix the bug or implement new features to the code quite fast.

**You also need an opinion on the clearness of project deliverables.**
<iframe width="560" height="315" src="https://www.youtube.com/embed/9tC7p2UTLsU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

**You always need an external reviewer of your code
to keep it clear and understandable for all people.**

And I believe that professional developer is more interested and motivated
in a project not with the most fancy set of technologies,
and not even the most mission-critical software like Google, Amazon, or Tinder,
but that **designed with a strong discipline and maintainability in mind**.

I think it's time to become a designer,
not just a motivated hacker coding for new magical concepts or Google.
Because focusing on maintainability can improve you and your project to the next level,
**new technologies don't**, no matter what Google CEO says.
