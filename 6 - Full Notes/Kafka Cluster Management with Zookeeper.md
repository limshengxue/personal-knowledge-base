2025-06-05 08:43

Tags: [[kafka]] [[zookeeper]]

# Kafka Cluster Management with Zookeeper
## ZooKeeper
- Framework to manage information & provide services necessary for running distributed systems
- Primarily used by developers to create distributed applications
	- Users interact with ZooKeeper
- Example applications:
	- Kafka
	- Hadoop
	- Neo4j

### What it does
- Handling config
- System naming
- Sync across systems
- Services required by a group of systems
- A framework to prevent individual distributed application having custom version of services

## ZooKeeper and Kafka
- Kafka use ZooKeeper for cluster management
- Two files used
	- `config/zookeeper.properties` - for zookeeper setup
	- `config/server.properties` - more information for Kafka

## Kafka Start in 2 steps
- `bin/zookeeper-server-start.sh config/zookeeper.properties`
- `bin/kafka-server-start.sh config/server.properties`


# References
[[6 - Creating and Managing Kafka Cluster]]