2025-06-05 08:41

Tags:[[data streaming]] [[kafka]] [[flink]]

# Kafka Connect and Processing with Flink
- Kafka Connect integrates Kafka with external data systems using connectors.
- Its responsibility is data movement, rather than arbitrary stateful stream computation.

## Source and Sink Connectors
- A source connector reads an external system and writes records to [[Kafka Topics and Messages]].
- A sink connector reads Kafka records and writes an external destination.
- Workers run configured connectors and tasks; deployment and connector support determine their behavior.

## Single Message Transforms
- SMTs apply stateless transformations to individual connector records.
- Examples include filtering, adding fields, and extracting a key.
- They are not a substitute for joins, aggregations, or state accumulated across events.

## When Processing Is Needed
Use [[Flink Stream Processing]] for the source's discussion of stateful stream operations and API selection. Other processing frameworks may also fit the workload.

# References
[[5 - Kafka Connect, Stream Processing]]
