---
layout: post
date: 2023-07-09
place: Minsk, Belarus
title: "Scalar Driven Development"
tags: oop java
---

It's very hard to keep small and organized interfaces.
Scalar pattern makes interfaces simple, maintainable and 
easy to use for their clients.

<!--more-->

<img src="/images/2023/07/saul-take.png">

## Problem

Let's say your interface looks like this: 

```java
interface Broker {
  Broker withTopics(TopicDirs topics);
  Broker withDatasets(DatasetDirs datasets);
  Broker withSubs(SubDirs subs);
}
```

`Broker` interface is not scalable at all.
On each new logic, new method will be added to it.
At some point, your interface ends up with 100+ methods.
It's a default problem now, thanks to Spring and Apache.

## Alternative

To refactor this interface, **we need to generalize all methods' parameters**.
Here I'm using `org.cactoos.Scalar` from [**cactoos**](https://github.com/yegor256/cactoos).

```java
interface Inputs extends Scalar<Directives> {
  @Override
  Directives value();
}
```

Now, all parameters: `TopicDirs`, `DatasetDirs`, `SubDirs`
will implement `Inputs` interface.

```java
final class TopicDirs implements Inputs {
  @Override
  public Directives value() {
    return new Directives("...ADD Topic...");
  }
}

final class DatasetDirs implements Inputs {
  @Override
  public Directives value() {
    return new Directives("...ADD Datasets...");
  }
}

final class SubsDirs implements Inputs {
  @Override
  public Directives value() {
    return new Directives("...ADD Sub...");
  }
}
```

Now, `Broker` interface can have only one method `with()`:

```java
interface Broker {
  
  Broker with(Inputs inputs);
}
```

Now, scalar generic inputs **help clients of this interface use how they want**
**keeping objects small and cohesive enough**.

You even can combine scalars into bigger one, using decorators:
```java
final class Combined implements Inputs {

  private final Inputs origin;

  @Override
  public Directives value() {
    return new Directives(
      "...ADD Combined...",
      this.origin.value()
    );
  }
}
```

Here is the use-case of this pattern in eo-kafka, [**sources**](https://github.com/eo-cqrs/eo-kafka),
(simplified):
```java
public final class FkConsumer implements Consumer<Object, String> {
  private final UUID id;
  private final Broker broker;

  @Override
  public void subscribe(final Collection<String> topics) {
    this.broker.with(new SubscribeDirs(topics, this.id).value());
  }

  @Override
  public void subscribe(final ConsumerRebalanceListener listener,
                        final String... topics) {
    this.broker.with(
      new WithRebalanceListener(
        new SubscribeDirs(t, this.id),
        listener
      ).value()
    );
  }

  @Override
  public ConsumerRecords<Object, String> records(
    final String topic, final Duration timeout
  ) throws Exception {
    this.broker.with(new SubscribeDirs(topic, this.id).value());
    this.broker.with(new SeenDirs(topic, rec.value()).value());
    return records;
  }

  @Override
  public void unsubscribe() throws Exception {
    this.broker.with(new UnsubscribeDirs(this.id).value());
  }
}
```

As you can see, intermediate Broker can be used in various scenarios,
keeping this interface simple and small.

## Avoiding Workers

To avoid [**evil**](https://www.yegor256.com/2015/03/09/objects-end-with-er.html)
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
