---
layout: post
date: 2023-02-18
place: Minsk, Belarus
title: "What is Reactive Programming?"
tags: reactive
---

Do you know what the main purpose of **```reactive```** programming?
I ask this question mainly when interview software engineers on Senior/Lead positions,
on projects, that require [**WebFlux
**](https://docs.spring.io/spring-framework/docs/current/reference/html/web-reactive.html)
or [**Reactor**](https://projectreactor.io) skills.
Very often I hear that reactive means non-blocking and performant.
So, it is not what the reactive programming **main purpose** is about.

<!--more-->

<img src="/images/2023/02/we-are-friends.png">

# Performant?

Also read about concurrency vs. parallelism.

# Non-blocking?

Most people starting with term of non-blocking.
By the way, non-blocking means asynchronous.
Non-blocking means that program doesn't block the execution of further operations, while
asynchronous - code will be executed not line by line.
So, [```Mono<T>```](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html) is non-blocking?
Yes,
but [```CompletableFuture<T>```](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html)
will be non-blocking and asynchronous, so non-blocking is not the main
feature that we needed using Reactive Programming.
By the way asynchronous data processing is not the mandatory part of the reactive streams.

I think that reactive programming main idea consists of 3 elements:

## Backpressure

Backpressure is the core part of reactive programming and [**reactive-streams**](http://www.reactive-streams.org).

# Data immutability

Reactive systems always have these roles:

- Subscriber/Consumer
- Processor
- Publisher/Producer
  The reactive programming idea is all around the data.
  It allows to handle data streams more effectively in a non-blocking way (where possible).
  Also, reactive programming designed with immutability in mind,
  so you execution starts only when consumer **subscribes** on publisher's data.

```java
public interface Publisher<T> {
  void subscribe(Subscriber<? super T> s);
}

public interface Subscriber<T> {
  void onSubscribe(Subscription s);

  void onNext(T t);

  void onError(Throwable t);

  void onComplete();
}
```

## Resources usage

Using non-blocking approach we allow to process data simultaneously in a one thread(event loop):
we just asks event loop thread for the next chunk of data to process, and it doesn't create new threads on each request.
Like [**Netty**]()(event loop) vs. [**Tomcat**]() (thread per request).
So, it increases resource consumption, and may help to reduce the costs.