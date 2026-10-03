2025-06-05 08:43

Tags: [[kafka]] [[zookeeper]]

# Kafka Cluster Management with Zookeeper
- Cluster metadata management depends on the Kafka release and deployment mode.
- Kafka 4.0 and later support KRaft, not ZooKeeper mode.
- This note preserves the source's older architecture as historical context rather than current setup instructions.

## Current KRaft Mode
- Controller quorum members manage cluster metadata using Kafka's Raft-based system.
- Broker and controller roles, storage formatting, and quorum configuration must follow the installed release's documentation.
- Do not start a ZooKeeper service as a prerequisite for Kafka 4.
- Distinguish cluster metadata from the event data in [[Kafka Topics and Messages]] and [[Kafka Partition, Brokers, and Replication]].

## Legacy ZooKeeper Mode
- Older Kafka deployments used an external [[zookeeper|ZooKeeper]] ensemble for coordination and metadata.
- ZooKeeper is a general coordination service, not a replacement for Kafka's event log.
- Historical installations used `config/zookeeper.properties` and `bin/zookeeper-server-start.sh`.
- These file names and startup commands are not a Kafka 4 installation recipe.
- Migration from an older ZooKeeper-mode cluster requires the supported intermediate versions and migration procedure; changing a startup command is insufficient.

# References
[[6 - Creating and Managing Kafka Cluster]]
[Kafka upgrade and KRaft requirements](https://kafka.apache.org/42/getting-started/upgrade/)
