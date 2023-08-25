---
layout: post
date: 2023-07-09
place: Minsk, Belarus
title: "Scalar Driven Development"
tags: oop java
---

It's very hard to make objects declarative.
Scalar Driven Development can help you to model
your objects as declarative parts of application.

<!--more-->

<img src="/images/2023/07/saul-take.png">

First things first.
Imperative programming covers 99% of code.
We're still writing the same procedural code, that we did in C 50 years ago.
We are done with it.
It's time to cure our brains.

## Scalars are Declarative

Scalar Driven Development is a way to model our objects as declarative parts,
some results, that will be computed later.

We define the interface first, where we denote our Scalar object.

Here is an example:

```java
interface User extends Scalar<Json>{
  @Override
  Json value();
}
```

Our Scalar from User is a JSON document.
By the way, `Scalar<T>` taken from [**cactoos library**](https://github.com/yegor256/cactoos).

Then we are creating a few implementations for it:
```java
final class Simple implements User {
  
  private final String username;
  
  @Override
  public Json value() {
    return new MutableJson().with("urn", this.username);
  }
}
```

No big deal, just giving username and building a JSON document
from it.
Someday, the logic will scale or change completely.
But we want our object to stay the same, while adding new features.
The [**Decorator**]() will be in charge:

```java
final class Validated implements User {
  
  private final Scalar<Json> user;
  
  @Override
  public Json value() {
    // validate json
    return this.user.value();
  }
}
```

Now we can validate our username before applying it to JSON document storage.
Frankly speaking, it's not a plain decorator, we are chaining a scalar by passing it.

```java
final class Flushed implements User {

  private final Scalar<Json> user;
  private final Documents docs;

  @Override
  public Json value() {
    this.docs.apply(this.user.value());
  }
}
```

That's a composition of objects, where each of them
is a good citizen of a pipeline.
Each of them is represented as a small result,
as a part of a bigger one.

```java
new Flushed(
  new Validated(
    new Simple(
      "Aliaksei"
    )
  ),
  docs  
).value(); // nothing will be computed until value() is called
```

## Scalars vs. Workers

`XmlParser`, `BookMapper`, `UserValidator`.
Who are they?!
I think they are great candidates for refactoring...

Scalars also will help you to avoid [**evil**](https://www.yegor256.com/2015/03/09/objects-end-with-er.html)
suffix of your objects: `-ER`,
a.k.a Worker, we just need to **define our results of manipulations with objects
as other objects**, that's how Scalar is born.

> "_Declarative programming
  means you define results of manipulations
  with objects as other objects_".

Let's see an example: we need to map `HttpRequest` to `Song` object: 

```java
interface Mapper<F, T> {
  T to(F from);
}

final class SongMapper implements Mapper<HttpRequest, Song> {
  @Override
  public Song to(final HttpRequest rq) {
    return new Song(rq.path("name"));
  }
}

// call it
final SongMapper mapper = new SongMapper();
final Song song = mapper.to(request);
```
`SongMapper` encapsulates nothing, frankly speaking
it's not even a class, it's nothing more than a set of procedures.
More than that, `SongMapper`
represent nothing from a [**real-world**](https://www.yegor256.com/2014/11/20/seven-virtues-of-good-object.html#1-he-exists-in-real-life).
We are just treating it like some smart object to manage our dumb DTOs.

Instead of doing such procedural things, we can go with SDD; 
it will make our objects more declarative:

```java
final class SongFromHttp implements Scalar<Song> {
  
  private final HttpRequest request;
  
  @Override
  public Song value() {
    return new Song(this.request.path("name"));
  }
}

// call it
final Song song = new SongFromHttp(request).value();
```

The same functionality, but now:

1. we are treating `SongFromHttp`as real-world entity.
2. `SongFromHttp` has a state inside self: object encapsulates `HttpRequest`
3. thanks to 2., we can perform lazy computation of `SongFromHttp#value()`

Watch this:

<iframe width="560" height="315" src="https://www.youtube.com/embed/6GMiosTLUTc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

**P.S**
<br>
[**@l3r8yJ**](https://www.l3r8y.ru/) called this way of development - **Scalar Driven Development (SDD)**.
