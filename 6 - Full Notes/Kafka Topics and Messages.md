2025-06-05 08:29

Tags: [[data streaming]] [[kafka]]

# Kafka Topics and Messages
## Topics
- Messages in Kafka are organized in Topics
- We can derive topics from other topics 
	- Example: Select reading > X from 1 topic
- Can manage using `bin/kafka-topics.sh`
- *Be careful*: when automatic topic creation is enabled and permitted, a misspelled topic can be created instead of producing an obvious missing-topic error.


![[Attachments/Pasted image 20250528085854.png]]

## Kafka Message
- Value: Can be JSON, integer, raw string, ...
- Key: Not mandatory. Choose carefully as it helps distributed data
- Timestamp
- Header: Key-value pairs
- Topic 
- Partition
- Offset


## Topic Names and Accidental Creation
- A misspelled topic name can create an unintended topic when broker auto-creation and applicable client behaviour permit it.
- Broker configuration `auto.create.topics.enable` controls server-side automatic creation; do not assume it is enabled in every environment.
- Disabling automatic creation does not replace topic provisioning, permissions, or validation. [Broker topic-creation setting](https://kafka.apache.org/41/configuration/broker-configs/#auto.create.topics.enable).

## Check Before Producing
For a Kafka distribution with command-line tools, use:

```bash
bin/kafka-topics.sh --bootstrap-server localhost:9092 --list
bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic thermostat_readings
```

- Replace the endpoint with one reachable from the client.
- Confirm exact spelling, partition configuration, and expected topic ownership.
- Provision topics explicitly when reproducible infrastructure is required.
- If data appears missing, compare the producer's configured topic with the consumer's subscription before investigating offsets.
- Unexpected topic names and client connection problems are different issues; see [[Kafka Producers and Consumers]].

# References
[[2 - Source Materials/Videos/DataCamp Course - Kafka/1 - Introduction, Topics, Messages|1 - Introduction, Topics, Messages]]
[[2 - Source Materials/Videos/DataCamp Course - Kafka/7 - Common Issues and Troubleshooting|7 - Common Issues and Troubleshooting]]
