---
layout: post
date: 2023-03-26
place: Minsk, Belarus
title: "EO-CQRS/eo-kafka - Kafka Producers and Consumers for Elegant Microservices"
tags: pets microservices
---

[EO-CQRS]() is a set of components for creating elegant microservices we started to work on just a few weeks ago.
The intent was to propose a clean and more declarative alternative to Spring, Eventuate, Project Reactor, and others.
Instead of calling static procedures, creating configuration classes, services, etc. we want to use clean and simple objects, the way they are supposed to be used.
Today we will see how Kafka producers and consumers work in an elegant, object-oriented way.

<!--more-->

<img src="/images/2023/03/batman-begins.png">

Let's say we want to create a Kafka Producer. 
This is how you would do it with the [Spring Framework]() Kafka starter:
```java
@Configuration
public class KafkaProducerConfig {

  @Bean
  public ProducerFactory<String, String> producerFactory() {
    Map<String, Object> configProps = new HashMap<>();
    configProps.put(
      ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, 
      bootstrapAddress);
    configProps.put(
      ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, 
      StringSerializer.class);
    configProps.put(
      ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, 
      StringSerializer.class);
    return new DefaultKafkaProducerFactory<>(configProps);
  }

  @Bean
  public KafkaTemplate<String, String> kafkaTemplate() {
    return new KafkaTemplate<>(producerFactory());
  }
}
```
```java
@Autowired
private KafkaTemplate<String, String> kafkaTemplate;

  public void sendMessage(String msg) {
    kafkaTemplate.send(topicName, msg);
}
```
```java
public void sendMessage(String message) {
  CompletableFuture<SendResult<String, String>> future = kafkaTemplate.send(topicName, message);
  future.whenComplete((result, ex) -> {
    if (ex == null) {
      System.out.println("Sent message=[" + message + 
  "] with offset=[" + result.getRecordMetadata().offset() + "]");
    } else {
      System.out.println("Unable to send message=[" + 
  message + "] due to : " + ex.getMessage());
    }
  });
}
```

This code is very imperative, also, very hard to test and reuse, thus it can lead to disaster in your software project.

This is how you can do it with [eo-kafka]():
1. With XML config file:
```java
    final Producer<String, String> producer =
      new KfProducer<>(
        new KfXmlFlexible<String, String>("settings.xml")
          .producer()
      );
```
2. Or with just passing the parameters which you need:
```java
    final Producer<String, String> producer =
      new KfProducer<>(
        new KfFlexible<>(
          new KfProducerParams(
            new KfParams(
              new BootstrapServers("localhost:9092"),
              new KeySerializer("org.apache.kafka.common.serialization.StringSerializer"),
              new ValueSerializer("org.apache.kafka.common.serialization.StringSerializer")
            )
          )
        )
      );
```
[//]: # (kafka producer sends the message)
This solution is actually cleaner, more declarative, testable, and in the end: more [maintainable]().
It perfectly decouples concepts, while the procedural one keeps them together.

Let's see what we can do with Kafka Consumer:

[//]: # (kafka consumer api)

As you see, object-oriented design helps to construct **cleaner**, **simpler**, more **maintainable**, **cohesive**, and **less coupled** software components.
All these things are virtues for microservices' architecture.
By the way, in EO-CQRS we even introduced 4 principles of 

[//]: # (4 principles)

[//]: # (its a good )
