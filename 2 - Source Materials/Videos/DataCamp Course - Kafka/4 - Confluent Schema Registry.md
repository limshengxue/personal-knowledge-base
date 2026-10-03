# Why need Schema Registry
- Consumer will expand (more people find the data useful)
- Schema will evolve

# Schema Registry
- Stand-alone machine run outside from Kafka cluster
- Maintain a database of all schema written to the cluster which it is responsible for
- Producers register schema with the registry before publishing to the cluster
- Produce take the schema ID obtained from the registry and publish the message together with the schema ID
- Consumer use the schema ID from the message to retrieve schema from the registry

![[Attachments/Pasted image 20250528101632.png]]
