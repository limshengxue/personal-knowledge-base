# Kafka Connect
- Talk to things that is not Kafka to take in data into Kafka
- Kafka Integration API
- Source Connector - read data from external system, write to topic
- Sink Connector - consume from topic, write to external system

## Single Message Transform
- They must be stateless
- Filter, Add field, Extract something as a key

# Stream Processing
- Consumer grow in complexity
- Stateful operation required like aggregation, joining, ...
- Flink has become the defacto standard form stream processing with Kafka

## Flink API
- DataStream API
	- Low level, not recommended for new design
- Table API
	- In Java/Python, SQL-ish
- SQL API
	- In SQL

 
