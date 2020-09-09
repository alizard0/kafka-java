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

**Maven Dependency**
```
<dependency>
  <groupId>org.springframework.kafka</groupId>
  <artifactId>spring-kafka</artifactId>
  <version>2.6.0</version>
</dependency>
```
**Consumer Properties**
```
public Map<String, Object> consumerProps() {
    Map<String, Object> props = new HashMap<>();
    props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
    props.put(ConsumerConfig.GROUP_ID_CONFIG, group);
    props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, true);
    props.put(ConsumerConfig.AUTO_COMMIT_INTERVAL_MS_CONFIG, "100");
    props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, "15000");
    props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, IntegerDeserializer.class);
    props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
    return props;
}
```
**Producer Properties**
```
public Map<String, Object> producerProps() {
Map<String, Object> props = new HashMap<>();
    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
    props.put(ProducerConfig.RETRIES_CONFIG, 0);
    props.put(ProducerConfig.BATCH_SIZE_CONFIG, 16384);
    props.put(ProducerConfig.LINGER_MS_CONFIG, 1);
    props.put(ProducerConfig.BUFFER_MEMORY_CONFIG, 33554432);
    props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, IntegerSerializer.class);
    props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
    return props;
}
```
**Publish Events**
```
@EnableKafka
public class KafkaConfig {
    @Bean
    public Map<String, Object> producerConfigs() {
        // your properties
    }
    
    @Bean
    public ProducerFactory<Integer, String> producerFactory() {
        return new DefaultKafkaProducerFactory<>(producerConfigs());
    }

    @Bean
    public KafkaTemplate<Integer, String> kafkaTemplate() {
        return new KafkaTemplate<Integer, String>(producerFactory());
    }
}

public class KafkaFacade {
    @Autowired
    KafkaTemplate<Intenger, String> template;
    
    public publishEvent(final MyEvent evt) {
        ProducerRecord<String, String> record = buildRecord(evt);
        template.send(record);
    }
}
```
**Event Listener**
```
public class MyEventListener {
    @KafkaListener(topics = {"my.topic.name"}, groupId = "my.kafka.group.id")
    public void handle(@Payload MyEvent event) {
        // your business logic
    }
}
```
