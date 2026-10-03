2025-06-05 08:29

Tags: [[data streaming]] [[kafka]]

# Kafka Topics and Messages
## Topics
- Messages in Kafka are organized in Topics
- We can derive topics from other topics 
	- Example: Select reading > X from 1 topic
- Can manage using `bin/kafka-topics.sh`
- *Be careful*: when writing message into a topic, Kafka will create a new topic if the topic name does not existed, so a typo in topic name will not result in error


![[Attachments/Pasted image 20250528085854.png]]

## Kafka Message
- Value: Can be JSON, integer, raw string, ...
- Key: Not mandatory. Choose carefully as it helps distributed data
- Timestamp
- Header: Key-value pairs
- Topic 
- Partition
- Offset


# References
[[1 - Introduction, Topics, Messages]]
