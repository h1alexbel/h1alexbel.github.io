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

**P.S**
<br>
[**@l3r8yJ**](https://www.l3r8y.ru/) called this way of development - **Scalar Driven Development (SDD)**.
