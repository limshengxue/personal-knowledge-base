2026-03-15 15:57

Tags: [[redis]]

# Redis Key and Value
## Value
- a piece of data that need to be stored
- can be text, number, ...

## Key
- Name or label for the value

### Updating Key-Value in CLI
`SET color red`
`GET color`
`UNLINK color` - return 1 if deleted, return 0 if no such key

### Naming Keys Conventions
- Use Keyspaces
	- Grouped related keys together using hierarchical set of prefix
	- Eg: "product:6379:color"
- Take advantage of ID in the app

### Types of Key
- Persistent key
	- Stay around until removed
	- Keys are persistent by default
- Volatile key
	- Live until TTL (automatically removed)
	- `EXPIRE product:name "apple" 30` - set TTL to 30 seconds
	- `TTL product:name` - check if key still alive, -1 alive, -2 not alive
	- Use Cases
		- Caching
		- Session management

## Strings
- Binary-safe sequence of bytes
- String can store different things in Redis
	- Plain text
	- Integer
	- Binary
	- CSV
	- JSON
	- Serialized objects
	- Binary data video, docs, audio, ...



# References
[[Use Key Expiration]]
[[Redis Key and Value]]