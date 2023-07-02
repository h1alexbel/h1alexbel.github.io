---
layout: post
date: 2023-07-02
place: Minsk, Belarus
title: "Degradation of Programming Languages"
tags: future
---

I am coding in Java and TypeScript.
I am creating backend applications using Java platform and a bit of Node.Js.
These languages and related platforms are being created in 1995 (Java) and 2012 (TypeScript).
Do we change something?
17 years are gone, more complex problems to solve, but
the same level of abstraction we have in programming languages and platforms.
It's time to change our tooling.

<!--more-->

<img src="/images/2023/07/gone-girl.jpg">

Now, in 2023, modern programming languages are focusing only on one thing:
<br>
_How to satisfy all the developers in the world, that they can use only our language to solve all their problems?_

The Biggest problem we have now,
that the languages we're dealing with are the products of the majority votes.
Do we need reflection and annotations?
Most developers will say "yes", no matter how bad is reflection.
The same with static methods.
The same with 90% of features in "modern" programming languages
such as Java, Kotlin, TypeScript, Go, Ruby, etc.

While languages and platforms formulate the [**maintainability**](https://h1alexbel.github.io/2023/06/25/maintainability-first-others-second.html) of the programs we create,
modern languages start looking uglier, uglier, and uglier.
To satisfy all the needs, modern languages inevitably start introduce
ability to write the same things in a different way, a.k.a flexibility, or [**syntactic sugar**](https://www.yegor256.com/2017/04/11/flexibility-equates-lower-quality.html).

> _Simply put, higher quality comes from simpler languages. - Yegor Bugayenko_

Just look at [**JEP 445: Unnamed Classes and Instance Main Methods**](https://openjdk.org/jeps/445), the Java 21 preview feature: 

Java main method and class a.k.a entry point can be presented now in 2 ways:

```java
public class Entry {
  public static void main(String[] args) {
    // logic
  }
}
```

```java
void main() {
  // logic
}
```

The same happens with all the languages.
**They introduce new and new features to satisfy all the developers around the world
and boost their creators on the stock market (Google, Microsoft)**.

## What is the solution?

I believe that now, we need more high-level languages with focus on specific use-cases.
It should be **simple**, **high-level**, and **specific-focused**.
That's it.

There are some features of really new and modern programming language:

1. VSC support at language level
2. UML and OCL support at language level
3. CI/CD support at language level
4. Declarative programming
5. Distributed objects/functions and their scalability
6. Cloud Computing
7. Data formats e.g., XML, JSON, YAML, CSV
8. Testing: unit, integration, end-to-end, performance
9. Multithreading
10. Documentation 
11. Software licenses 
12. Low-code compatibility 
13. Runtime platforms e.g., Docker, K8s 
14. Object Metadata
15. Object metrics 
16. Component repository: e.g., Sonatype Nexus 
17. Design patterns indication 
18. Compilation into other programming languages 
19. No scalar types e.g., `int`, `boolean`
20. No data flow control e.g., `for`, `while`
21. No reflection
