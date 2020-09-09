# Kafka
> a distributed streaming platform

For official documentation, visit https://kafka.apache.org/documentation/#gettingStarted

**Event Streaming** is the practice of capturing, storing and processing data in real-time from multiple event sources like databases, services, applications, sensors and so on. Nowadays event-driven architectures are used in different use cases, namely: payment processing, financial transactions, IoT systems, monitoring, event-driven architectures, data platforms etc..

**Kafka** is a event streaming platform that provides three key features which allow developers to implement event streaming architectures. These features are:

1. to publish and to subscribe event streams (topics)
2. to store event streams durably and reliably
3. to process event streams

**Event** represents something that happened recently or in the past in your business. Kafka Events have a key, value, timestamp and metadata headers.

**Producer** is a Kafka client that publishes events into topics and **consumer** is a Kafka client that reads events from topics. As both clients interact with a topic, they will never block each other to access the resources.

In short, events are sorted and persisted in topics which works like a filesystem. In addition, topics are **partitioned** which means that a topic is spread over a number of buckets on different kafka-brokers. This design allows client applications to both read from and write to may brokers at the same time. Events with the same event key are written in the same partition.


## Spring-Kafka

For official documentation, visit https://docs.spring.io/spring-kafka/reference/html/

**Configuration**

**Event Listeners**

**Publish Events**