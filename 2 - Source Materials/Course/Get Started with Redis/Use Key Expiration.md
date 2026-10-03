2026-03-15 15:52

# Use Key Expiration
2 types of key
- Persistent key
	- Stay around until removed
	- Keys are persistent by default
- Volatile key
	- Live until TTL (automatically removed)
	- `EXPIRE product:name "apple" 30` - set TTL to 30 seconds
	- `TTL product:name` - check if key still alive, -1 alive, -2 not alive

## Use Cases
- Caching
- Session management


# References
