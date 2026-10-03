2026-10-03 18:49

Tags: [[data streaming]] [[kafka]] [[flink]]

# Flink Stream Processing
- Stream processing computes results from events as they arrive.
- Stateful operations such as aggregation, joins, and event-time windows require more than a stateless per-message transform.
- Flink is one option for these workloads, not a mandatory or universally dominant Kafka processor.

## API Choices
- SQL and Table API express relational transformations declaratively.
- DataStream API gives finer control over state, timers, and processing behavior.
- Start with the abstraction matching the problem; DataStream remains appropriate when relational APIs do not express the required control.
- Supported language bindings and APIs vary by Flink release.

## Kafka Integration
- Consume events from [[Kafka Topics and Messages]] and write results to a sink.
- Decide how checkpoints, source offsets, and sink behavior affect recovery and delivery guarantees.
- State includes data that must survive failure or be reconstructed; plan its size and lifecycle.
- Event time, processing time, and late-event handling are different choices.

## Connect vs Processing
- [[Kafka Connect and Processing with Flink|Kafka Connect]] moves data between Kafka and external systems.
- Single Message Transforms operate on individual connector records and are stateless.
- Use a processing engine when computations depend on several events, accumulated state, or window boundaries.

# References
[[5 - Kafka Connect, Stream Processing]]
[Flink API selection](https://nightlies.apache.org/flink/flink-docs-stable/docs/dev/overview/)
