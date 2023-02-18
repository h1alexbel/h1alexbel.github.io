---
layout: post
date: 2023-02-18
place: Minsk, Belarus
title: "What is Reactive Programming?"
tags: reactive
---

Do you know what the main purpose of **```reactive```** programming?
I ask this question mainly when interview software engineers,
on projects, that require [**WebFlux**](https://docs.spring.io/spring-framework/docs/current/reference/html/web-reactive.html)
or [**Reactor**](https://projectreactor.io) skills.
Very often I hear that reactive means non-blocking and performant.
This is not far away from the truth. But reactive programming has more concepts on top of the non-blocking API.

<!--more-->

<img src="/images/2023/02/we-are-friends.png">

## Non-blocking?

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

Let's see which concepts reactive programming has on top of the non-blocking execution:

## Backpressure

Backpressure is the core part of reactive programming and [**reactive-streams**](http://www.reactive-streams.org).
The backpressure – is a mechanism of requesting data by consumer using buffering or dropping that data.
So, producer controls data flow and consumer uses backpressure in order to be not overwhelmed by the traffic.

## Data immutability

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
```
```java
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
Like [**Netty**](https://netty.io)(event loop) vs. [**Tomcat**](https://tomcat.apache.org) (thread per request).
Moreover, data processing happens only after **subscription** was made.
So, it increases resource consumption, and may help to reduce the costs.