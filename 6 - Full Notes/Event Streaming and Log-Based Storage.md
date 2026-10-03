2025-06-05 08:23

Tags: [[data streaming]]

# Event Streaming and Log-Based Storage
## Event Streaming is about
- Obtain source information
- Reliably storing
- Distribute data to client
- Real-time processing: process event as soon as they occur

## Data Model 
- Kafka treat data as event/messages instead of *thing* (like a database)
- Events example include like user click, certain trigger, sensor readings
- Real-time processing: process event as soon as they occur

### Log-Based Storage
- Traditional database store data in tables
- Problem: Lost context, losing historical record (missing log-type data)
- Streaming tools like Kafka used log-based storage
	- Messages/events stored organized with topic
	- Messages are immutable
	- Messages are naturally schema-less

### Log vs Queue
- Logs can read repetitively. But event can only be consumed once.

### Features of Log
- Log retention:
	- Delete old data or trim by size
- Log compaction
	- Keep only latest value per key


# References
[[1 - Introduction, Topics, Messages]]