2025-06-05 08:31

Tags: [[data streaming]] [[kafka]]

# Kafka Partition, Brokers, and Replication
## Partition
- For scalability, a topic should not be limited by the capacity of a single node
- Kafka split a topic into multiple partition 

### How to Distribute Message
- Message without key: Round-robin distribution (no guarantee order)
- Message with key: The key pass through a hash and mod function to decide which partition to store the message

## Brokers
- Kafka composed of a network of machines known as *broker* running Kafka server
- Abstracted away when using managed service like Confluent Cloud
- Brokers formed Kafka Cluster
- Each broker hold different partitions
- Broker handle request to write new message to the partition and read messages from the partition

## Replication
- For fault tolerance
- Choose a replication factor
	- Eg. "3" means each partition replicate 3 times across different brokers
- Leader replica handle the read/write, follower just sync with the leader
- But we can configure to read from the nearest follower (for performance)
- When broker failure, new leader will be elected
- Replication - 1 = number of failures we can handle
- Max replication factor = number of nodes (brokers)
- Use `home/repl/check_broker.sh` to check the number of brokers


# References
[[2 - Partition, Brokers, Replication]]