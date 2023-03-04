---
layout: post
date: 2023-03-04
place: Minsk, Belarus
title: "Event-Driven Architecture, and Why Its so Complex"
tags: microservices
---

Preferring microservices to monolith architecture means more challenges.
Most difficult ones are: distributed transactions, distributed locking, communication via message broker,
and of course enabling events. Most of these problems have well-known solutions named patterns.
CQRS, Saga, Event sourcing. They all referring to event-driven architecture,
and unfortunately, they also increase the complexity of the entire system.
So increasing complexity we are making our software less maintainable.
Can we design our microservices better?

<!--more-->

<img src="/images/2023/03/broen-clown.png">

To answer on this question firstly we need to dive deeply into event-driven architecture and its components.

<img src="/images/2023/03/eda.png">

## Events
<img src="/images/2023/03/events.png">

Each event is **immutable**, which is good for maintainability.
The same as [**object**](https://www.yegor256.com/2014/11/20/seven-virtues-of-good-object.html), good event should be small and immutable.
To read the data we are composing events into projections and show then.
Simple example:
UserAdded:
```json
{
  "eventType": "UserAdded",
  "id" : "256",
  "payload" : {
    "username" : "yegor256"
  }
}
```

UserChangeName:
```json
{
  "eventType": "UserChangeName",
  "id" : "256",
  "payload" : {
    "username" : "YEGOR256"
  }
}
```

User projection:
```java
final class User {
  private final Long id;
  private final String username;
}
```

Events enabling read/write flexibility, ability to form event/data streaming,
and the most important: your application's state is consists of the events, that you store.

## Event store
Event store is the place where we store all the events.
Usually that's a databases.

## Event bus
Event bus is just a pipeline of events.
Like [**Kafka**](https://kafka.apache.org), [**RabbitMQ**](https://www.rabbitmq.com), or [**SQS**](https://aws.amazon.com/sqs/).

## Event sourcing
"The fundamental idea of Event Sourcing is that of
ensuring every change to the state of an application is
captured in an event object, and that these event
objects are themselves stored in the sequence they
were applied for the same lifetime as the application
state itself." - [**Martin Fowler**](https://martinfowler.com)
Simply put, event sourcing - is the process of persisting business entities as a sequence
of events.

## CQRS
```CQRS``` is an architectural pattern acronym, standing for Command Query Responsibility Segregation.
It divides a system’s actions into commands and queries.
It is related to ```CQS```, which is Command Query Separation.
```CQS``` is an architectural pattern, and the acronym stands for Command Query Separation.
CQS is the core concept that defines two types of operations handled in a system: a command that executes a task,
a query that returns information, and there should never be one function to do both of these jobs.
The term was created by [**Bertrand Meyer**](https://en.wikipedia.org/wiki/Bertrand_Meyer) in his book [**Object-oriented Software Construction**](https://www.amazon.com/Object-Oriented-Software-Construction-Book-CD-ROM/dp/0136291554) (1988, Prentice Hall).

Also, we need some sort of distributed transactions:

## Saga
<img src="/images/2023/03/saga.png">

In microservices, each service has its own database.
Thus, business transactions should be also split into multiple services.
So, local transaction won't work.
In this case we need to design distributed transaction management and also,
compensating transactions too.
This pattern really brings complexity.
My advice is to **try to design less coupled services**.
Always think about transactional logic when you are making services decomposition.

Now to problems: **complexity** and **maintainability**.

Main complexity we got from frameworks that we are creating and using.
Our microservices are so complex and unmaintainable not because of Kafka or CQRS involvement.
But because we use such frameworks, that are designed without any ```simplicity``` and ```maintainability``` in mind.
Just look at the [**Spring Cloud**](https://spring.io/projects/spring-cloud) or more event-oriented [**Eventuate**](https://eventuate.io).

So, we need to form principles and then construct dedicated frameworks.

That's why I start designing [**EO-CQRS**](https://eo-cqrs.github.io/.github/), OOP event sourcing & cqrs framework,
that tries to reduce the complexity of microservices architecture.
And you can help by submitting issues and pull requests!