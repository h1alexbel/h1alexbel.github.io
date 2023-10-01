---
layout: post
date: 2023-03-26
place: Minsk, Belarus
title: "EO-CQRS/eo-kafka - Kafka Producers and Consumers for Elegant Microservices"
tags: [pets, microservices]
---

[EO-CQRS](https://github.com/eo-cqrs) is a set of components for creating elegant microservices we started to work on just a few weeks ago.
The intent was to propose a clean and more maintainable alternative to Spring, Eventuate, Project Reactor, and others.
Instead of calling static procedures, creating configuration classes, services, etc. we want to use clean and simple objects, the way they are supposed to be used.
Today we will see how Kafka producers and consumers work in an elegant, object-oriented way.

<!--more-->

<img src="/assets/images/2023/03/batman-begins.png">

Let's say we want to create a Kafka Producer. 
This is how you would do it with the [Spring Framework](https://spring.io/) Kafka starter:

```java

@Configuration
public class KafkaProducerConfig {

  @Bean
  public ProducerFactory<String, String> producerFactory() {
    final Map<String, Object> config = new HashMap<>();
    config.put(
      ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,
      bootstrapAddress);
    config.put(
      ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG,
      StringSerializer.class);
    config.put(
      ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG,
      StringSerializer.class);
    return new DefaultKafkaProducerFactory<>(config);
  }

  @Bean
  public KafkaTemplate<String, String> kafkaTemplate() {
    return new KafkaTemplate<>(this.producerFactory());
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

This code is very imperative, also, very hard to test and reuse, thus it can lead to disaster in your [software project]().

This is how you can do it with [eo-kafka](https://github.com/eo-cqrs/eo-kafka):
1. With XML config file:
```java
    final Producer<String, String> producer =
      new KfProducer<>(
        new KfXmlFlexible<String, String>("settings.xml")
          .producer()
      );
```

XML should be in resources dir, and has the following structure:
```xml
<producer>
  <bootstrapServers>cluster.svc:9092</bootstrapServers>
  <keySerializer>org.apache.kafka.common.serialization.StringSerializer</keySerializer>
  <valueSerializer>org.apache.kafka.common.serialization.StringSerializer</valueSerializer>
</producer>
```

2. Or with just passing the parameters which you need:
```java
    final Producer<String, String> producer =
      new KfProducer<>(
        new KfFlexible<>(
          new KfProducerParams(
            new KfParams(
              new BootstrapServers("cluster.svc:9092"),
              new KeySerializer("org.apache.kafka.common.serialization.StringSerializer"),
              new ValueSerializer("org.apache.kafka.common.serialization.StringSerializer")
            )
          )
        )
      );
```
[Here](https://github.com/eo-cqrs/eo-kafka#config-api) you can find the full mapping of Kafka properties, our classes for them, and XML tags for your files.


So far, so good, and now we can send message to the broker:
```java
try (final Producer<String, String> producer = ...) {
      producer.send(
        "key2012",
        new KfData<>(
          "newRest28",
          "orders",
          1
        )
      );
    } catch (Exception e) {
        throw new IllegalStateException(e);
  }
}
```

Let's see what we can do with Kafka Consumer:

Firstly, with Spring Kafka:

```java

@Configuration
public class KafkaConsumerConfig {

  @Bean
  public ConsumerFactory<String, String> consumerFactory() {
    final Map<String, Object> props = new HashMap<>();
    props.put(
      ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG,
      bootstrapAddress);
    props.put(
      ConsumerConfig.GROUP_ID_CONFIG,
      groupId);
    props.put(
      ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG,
      StringDeserializer.class);
    props.put(
      ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG,
      StringDeserializer.class);
    return new DefaultKafkaConsumerFactory<>(props);
  }

  @Bean
  public ConcurrentKafkaListenerContainerFactory<String, String>
  kafkaListenerContainerFactory() {
    final ConcurrentKafkaListenerContainerFactory<String, String> factory =
      new ConcurrentKafkaListenerContainerFactory<>();
    factory.setConsumerFactory(this.consumerFactory());
    return factory;
  }
}
```

```java
@KafkaListener(topics = "topicName", groupId = "foo")
public void listenGroupFoo(String message) {
    System.out.println("Received Message in group foo: " + message);
}
```

And with eo-kafka:

Again, using XML File approach:
```java
final Consumer<String, String> consumer =
  new KfConsumer<>(
    new KfXmlFlexible<String, String>("consumer.xml")
      .consumer()
  );
```

with following XML file structure:
```xml
<consumer>
  <bootstrapServers>cluster.svc:9092</bootstrapServers>
  <groupId>1</groupId>
  <keyDeserializer>org.apache.kafka.common.serialization.StringDeserializer</keyDeserializer>
  <valueDeserializer>org.apache.kafka.common.serialization.StringDeserializer</valueDeserializer>
</consumer>
```

Or with help of parameters:
```java
final Consumer<String, String> consumer =
  new KfConsumer<>(
    new KfFlexible<>(
      new KfConsumerParams(
        new KfParams(
          new BootstrapServers("cluster.svc:9092"),
          new GroupId("1"),
          new KeyDeserializer("org.apache.kafka.common.serialization.StringDeserializer"),
          new ValueDeserializer("org.apache.kafka.common.serialization.StringDeserializer")
        )
      )
    )
  );
```

Now, we can consume some messages from the broker:
```java
try (
  final Consumer<String, String> consumer =
      new KfConsumer<>(
        new KfFlexible<>(
          new KfConsumerParams(
            new KfParams(
              new BootstrapServers(this.severs),
              new GroupId("1"),
              new AutoOffsetReset("earliest"),
              new KeyDeserializer("org.apache.kafka.common.serialization.StringDeserializer"),
              new ValueDeserializer("org.apache.kafka.common.serialization.StringDeserializer")
            )
          )
        )
      )
  ) {
  // you need to be subscribed on a topic to iterate over data in the topic
      consumer.subscribe(new ListOf<>("orders-saga-init")));
      List<Dataized<String>> result = consumer.iterate("orders-saga-init", Duration.ofSeconds(5L));
    }
  }
```

By the way, you can create Kafka Consumer wrapping original [KafkaConsumer](https://kafka.apache.org/23/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html):
```java
KafkaConsumer origin = ...;
Consumer<String, String> producer = new KfConsumer<>(origin);
```

This solution is actually cleaner, more declarative, testable, and in the end: more [maintainable](https://www.yegor256.com/2018/09/12/clear-code.html).
It perfectly decouples concepts, while the procedural one keeps them together.

As you see, object-oriented design helps to construct **cleaner**, **simpler**, more **maintainable**, **cohesive**, and **less coupled** software components.
All these things are virtues for microservices' architecture.
By the way, in EO-CQRS we even introduced 4 principles of clean development process:
```text
1. Simplicity is the virtue
2. No monolithic repositories
3. No manual deployments
4. Master branch is read-only
```

If you have some ideas how we can improve the microservices architecture, please [submit](https://github.com/eo-cqrs/ideas/issues) the issue, and we will provide you feedback.