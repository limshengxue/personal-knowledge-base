2026-03-15 10:09

# Redis Keys, Values and Strings
## Value
- a piece of data that need to be stored
- can be text, number, ...

## Key
- Name or label for the value

### Updating Key-value in CLI
`SET color red`
`GET color`
`UNLINK color` - return 1 if deleted, return 0 if no such key

### Naming Keys Conventions
- Use Keyspaces
	- Grouped related keys together using hierarchical set of prefix
	- Eg: "product:6379:color"
- Take advantage of ID in the app


## String
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
