Kafka Watcher
---
[![GitHub Actions](https://github.com/jcasbin/kafka-watcher/actions/workflows/ci.yml/badge.svg)](https://github.com/jcasbin/kafka-watcher/actions/workflows/ci.yml)
![License](https://img.shields.io/github/license/jcasbin/kafka-watcher)

KafkaWatcher is a [Kafka](https://kafka.apache.org) watcher for [jCasbin](https://github.com/casbin/jcasbin).

## Installation

For Maven

There is no maven-central package, please download and package by yourself

## Simple Example

```java
public static void main(String[] args) {
    // Initialize the watcher.
    // Use the Kafka properties and topic name as parameter.
    Map<String,Object> producerProps = new HashMap<>();
    producerProps.put("bootstrap.servers", "127.0.0.1:9092");
    producerProps.put("acks", "all");
    producerProps.put("retries", 0);
    producerProps.put("batch.size", 16384);
    producerProps.put("linger.ms", 1);
    producerProps.put("buffer.memory", 33554432);
    producerProps.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
    producerProps.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

    Map<String,Object> consumerProps = new HashMap<>();
    consumerProps.put("bootstrap.servers", "127.0.0.1:9092");
    consumerProps.put("group.id", "my-consumer-group-01");
    consumerProps.put("enable.auto.commit", "true");
    consumerProps.put("auto.commit.interval.ms", "1000");
    consumerProps.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
    consumerProps.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
    
    String topic="jcasbin-topic";
    KafkaWatcher kafkaWatcher = new KafkaWatcher(producerProps,consumerProps, topic);

    // Initialize the enforcer.
    Enforcer enforcer = new Enforcer("examples/rbac_model.conf", "examples/rbac_policy.csv");

    // Set the watcher for the enforcer.
    enforcer.setWatcher(redisWatcher);

    // Set callback to local example
    Runnable updateCallback = ()->{
        //do something
    };
    kafkaWatcher.setUpdateCallback(updateCallback);

    // Update the policy to test the effect.
    enforcer.savePolicy();
}
```

## Getting Help

- [jCasbin](https://github.com/casbin/jCasbin)
- [kafka-client](https://github.com/apache/kafka)

## License

This project is under Apache 2.0 License. See the [LICENSE](https://github.com/jcasbin/kafka-watcher/blob/master/LICENSE) file for the full license text.
