2025-06-05 08:34

Tags: [[data streaming]] [[kafka]]

# Kafka Producers and Consumers
## Producers
- Client application that write to the Kafka cluster
- Kafka has built-in serialization for primitive but not object
- Producer partition selection depends on explicit partition, key, and configured partitioner. The default uses key hashing or sticky selection; see [[Kafka Partition, Brokers, and Replication]].

![[Attachments/Pasted image 20250528093242.png]]

### Types of Producers
- To view producer using cli `kafka-console-producer.sh`
	- 2 required options:
		- `--bootstrap-server <server_address>`
		- `--topic <topicname>`
- API for programming language (Python, Java, ...)
- Kafka Connect (take data from different sources)


## Consumers
- Consume data
- Kafka provides retained event logs with non-destructive reads; see [[Event Streaming and Log-Based Storage]].
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
}
```

### Types of consumers
- Command-line `kafka-console-consumer`
	- 2 required arguments
		- `--topic`
		- `--bootstrap-server`
	- Optional arguments
		- `--from-beginning`
		- `--max-messages`
- Programming language API
- Kafka Connect

### Consumer Offset Tracking/Commit
- A mechanism to keep track of the consumer status
- Allow fault tolerance
- A committed offset normally identifies the next record to read, not simply the last record received. Commit timing relative to processing affects redelivery and loss risks.
- The offset store in broker's internal topic
- When consumer failed and restart
	- It can continue where it left after failure, according to the commit offset stored

### Consumer Group
- We can scale up consumer into a cluster to meet the high traffic of multiple partitions


## Connectivity Troubleshooting
1. Confirm the broker process is running and listening on the configured interface and port.
2. Test reachability from the client's actual machine or container, not only from the broker host.
3. Check DNS, routes, firewall rules, TLS, and authentication settings.
4. Inspect metadata-advertised broker addresses as well as the bootstrap address.
5. Confirm the exact topic and the client's authorisation before investigating consumer offsets.

On Windows, a basic TCP check is:

```powershell
Test-NetConnection -ComputerName broker.example.internal -Port 9092
```

A successful TCP test does not prove Kafka authentication, metadata retrieval, or topic access.

## Bootstrap vs Advertised Addresses
- `bootstrap.servers` provides initial broker endpoints.
- Clients then use cluster metadata to connect to the brokers responsible for partitions.
- Broker `advertised.listeners` must contain addresses reachable from those clients.
- An advertised container-only hostname or loopback address can break subsequent connections even when bootstrap succeeds.
- Binding a listener and advertising an address are different configuration tasks. [Broker listener configuration](https://kafka.apache.org/41/configuration/broker-configs/#advertised.listeners).

## Inspect Without Exposing Secrets
- A local process or listening-port check proves only local state.
- Review client and broker errors, but redact credentials and sensitive message contents.
- Use [[Kafka Topics and Messages]] to rule out accidental topic creation or a subscription typo.

# References
[[2 - Source Materials/Videos/DataCamp Course - Kafka/3 - Producers, Consumers|3 - Producers, Consumers]]
[[2 - Source Materials/Videos/DataCamp Course - Kafka/7 - Common Issues and Troubleshooting|7 - Common Issues and Troubleshooting]]
[Producer configuration](https://kafka.apache.org/42/configuration/producer-configs/)
