---
layout: post
date: 2023-04-15
place: Minsk, Belarus
title: "EO-Kafka with Spring"
tags: [pets, microservices]
---

When building the microservices we need to use frameworks to boostrap the project.
Spring is one of them.
We are not going to discuss here problems with Spring Framework in general.
Instead, we will connect eo-kafka with Spring.
Have fun!

<!--more-->

<img src="/assets/images/2023/04/broen-murder.png">

[Here](https://h1alexbel.github.io/2023/03/26/eo-kafka-for-elegant-microservices.html) we discuss how to use EO-Kafka and how it's different from Spring Kafka starter.
In this blog post I will show you how to integrate EO-Kafka into existing Spring Application.

## 1. Add a dependency
```xml
<dependency>
  <groupId>io.github.eo-cqrs</groupId>
  <artifactId>eo-kafka</artifactId>
  <version><!--get the latest from maven central--></version>
  <exclusions>
    <exclusion>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-simple</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

## 2. Create config
You can create Beans using [Parameters API](https://github.com/eo-cqrs/eo-kafka#config-api):

```java
@Configuration
public class KafkaConfig {

  @Bean
  public Producer<String, User> userProducer() {
    return new KfProducer<>(
      new KfFlexible<>(
        new KfProducerParams(
          new KfParams(
            new BootstrapServers("< bootstrap servers >"),
            new KeySerializer("org.apache.kafka.common.serialization.StringSerializer"),
            new ValueSerializer("< user serializer >")
          )
        )
      )
    );
  }

  @Bean
  public Consumer<String, User> consumer() {
    return new KfConsumer<>(
      new KfFlexible<>(
        new KfConsumerParams(
          new KfParams(
            new BootstrapServers("< bootstrap servers >"),
            new GroupId("1"),
            new KeyDeserializer("org.apache.kafka.common.serialization.StringDeserializer"),
            new ValueDeserializer("< user deserializer >")
          )
        )
      )
    );
  }
}
```

If you want to store all the information about the Producers and Consumers XML can help you:

```java
@Configuration
public class KafkaConfig {

  @Bean
  public Producer<String, User> userProducer() throws Exception {
    return new KfProducer<>(
      new KfXmlFlexible<>(
        "userProducer.xml"
      )
    );
  }

  @Bean
  public Consumer<String, User> consumer() throws Exception {
    return new KfConsumer<>(
      new KfXmlFlexible<>(
        "userConsumer.xml"
      )
    );
  }
}
```

with following XML files:
`userProducer.xml`
```xml
<producer>
  <bootstrapServers><!--boostrap servers --></bootstrapServers>
  <keySerializer><!--key serializer--></keySerializer>
  <valueSerializer><!--user serializer--></valueSerializer>
  ...
</producer>
```

`userConsumer.xml`
```xml
<consumer>
  <bootstrapServers><!--bootstrap servers--></bootstrapServers>
  <groupId><!--group id--></groupId>
  <keyDeserializer><!--key deserializer--></keyDeserializer>
  <valueDeserializer><!--user deserializer--></valueDeserializer>
  ...
</consumer>
```

All parameters and it's tags is [here](https://github.com/eo-cqrs/eo-kafka#config-api).

By the way your files structure should look life this:
```text
src/
  main/
    java/
      foo/
        KafkaConfig.java
    resources/
        userConsumer.xml
        userProducer.xml
```

## 3. Use it!
Inject your `Consumer` and `Producer` and use it like I'm described [here](https://h1alexbel.github.io/2023/03/26/eo-kafka-for-elegant-microservices.html)

If you have some ideas how we can improve the microservices architecture,
please [submit](https://github.com/eo-cqrs/ideas/issues) the issue, and we will provide you feedback.