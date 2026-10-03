2025-06-05 08:38

Tags: [[data streaming]] [[kafka]] [[schema]]


# Kafka Confluent Schema Registry
## Why need Schema Registry
- Consumer will expand (more people find the data useful)
- Schema will evolve

## Schema Registry
- Stand-alone machine run outside from Kafka cluster
- Stores schemas explicitly registered with it; it does not automatically discover every message schema in the Kafka cluster.
- Compatible serializers can register schemas or use schemas registered beforehand.
- Producer take the schema ID obtained from the registry and publish the message together with the schema ID
- Consumer use the schema ID from the message to retrieve schema from the registry

![[Attachments/Pasted image 20250528101632.png]]


## Integration Boundary
- Registry-aware serialization adds the identifiers needed by compatible deserializers; the wire format depends on the serializer and version.
- Unrelated producers can write to [[Kafka Topics and Messages]] without using Schema Registry.
- The registry is a service and can use managed or distributed deployment, not necessarily one standalone machine.

# References
[[4 - Confluent Schema Registry]]
[[Kafka Producers and Consumers]]
[Schema registration and serialization](https://docs.confluent.io/platform/current/schema-registry/fundamentals/serdes-develop/index.html)
