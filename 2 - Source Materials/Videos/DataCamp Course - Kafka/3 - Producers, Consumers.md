# Producers
- Client application that write to the Kafka cluster
- Kafka has built-in serialization for primitive but not object
- The producers library perform the distribution (round-robin or hashing according to key)

![[Attachments/Pasted image 20250528093242.png]]

## Types of Producers
- Command line `kafka-console-producer.sh`
	- 2 required options:
		- `--bootstrap-server <server_address>`
		- `--topics <topicname>`
- API for programming language (Python, Java, ...)
- Kafka Connect (take data from different sources)


# Consumers
- Consume data
- Always remember Kafka provide log not queue (non-destructive reads)

```java
Properties config = loadProperties("kafka.properties");
KafkaConsumer<String, String> consumer = new KafkaConsumer<>(config);
consumer.subscribe(List.of("thermostat_readings"));
while (true) {
   ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(10));
   for (ConsumerRecord<String, String> record : records) {
      System.out.println("Message=" + record.key() + ":" + record.value());
      // further process the record...
}
```

## Types of consumers
- Command-line `kafka-console-consumer`
	- 2 required arguments
		- `--topic`
		- `--boostrap-server`
	- Optional arguments
		- `--from-beginning`
		- `--max-messages`
- Programming language API
- Kafka Connect

## Consumer Offset Tracking/Commit
- The consumer tell the broker which offset it received
- The offset store in broker's internal topic
- This allow fault tolerance
	- Consumer can continue where it left after failure

## Consumer Group
- We can scale up consumer to meet the high traffic of multiple partitions
