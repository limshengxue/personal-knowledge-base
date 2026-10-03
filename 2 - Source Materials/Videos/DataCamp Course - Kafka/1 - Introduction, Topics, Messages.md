# Event Streaming
- Obtain source information
- Reliably storing
- Distribute data to client

# Data Model
- Kafka treat data as event instead of *thing*
- Events like user click, certain trigger
- Real-time processing: process event as soon as they occur

# Topics
- Traditional database store data in tables
- Problem: Lost context, losing historical record (missing log-type data)
- Kafka used log-based storage
	- Messages/events stored organized with topic
	- Messages are immutable
	- Messages are naturally schema-less
- We can derive topics from other topics 
	- Example: Select reading > X from 1 topic
- Logs. Not queue
	- Queue. When read from queue, the message is not accessible anymore
	- Logs can read repetitively.
- Log retention:
	- Delete old data or trim by size
- Log compaction
	- Keep only latest value per key
- Can manage using `bin/kafka-topics.sh`

![[Attachments/Pasted image 20250528085854.png]]

## Kafka Message
- Value: Can be JSON, integer, raw string, ...
- Key: Not mandatory. Choose carefully as it helps distributed data
- Timestamp
- Header: Key-value pairs
- Topic 
- Partition
- Offset
