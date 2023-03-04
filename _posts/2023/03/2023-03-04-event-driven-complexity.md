---
layout: post
date: 2023-03-04
place: Minsk, Belarus
title: "Event-Driven Architecture, and why Its so Complex"
tags: microservices
---

Preferring microservices to monolith architecture means more challenges.
Most difficult ones are: distributed transactions, distributed locking, communication via message broker,
and of course ${event}. Most of these problems have well-known solutions named patterns.
CQRS, Saga, Event sourcing. They all referring to event-driven architecture,
and unfortunately, they also increase the complexity of the entire system.
So increasing complexity we are making our software less maintainable.
Can we design our microservices better?

<!--more-->

<img src="/images/2023/03/broen-clown.png">