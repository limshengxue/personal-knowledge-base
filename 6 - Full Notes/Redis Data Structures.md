2026-03-15 15:59

Tags: [[redis]]

# Redis Data Structures
## Lists
- An ordered group of string elements
- Head and tail (beginning and end of the list)
- Use Cases
	-  When order matters
	- As a message queue
	- As a stack

### Commands to Work With List
Add element from the head/tail
`LPUSH products:recent X`
`RPUSH products:recent X`

Check the length
`LLEN products:recent`

Remove element from tail/head
`RPOP products:recent`
`LPOP products:recent`

Get Element
`LRANGE products:recent 0 2` - a range of element
`LRANGE products:recent 0 -1` accept negative index
`LINDEX products:recent 1` - single element


## Sets
- Unordered collection of unique string elements
- Use Cases
	- Counting and tracking unique things
	- Deduplicate things

### Commands to work with Sets
`SADD product:views alice`
`SMEMBERS product:views` - view the members
`SCARD product:views` - view count of a set
`SREM product:views alice` - remove a member
`SUNION` - union
`SINTER` - intersect
`SDIFF` - different between sets (order matters for this operation)


## Sorted Sets
A set where each member is associated with a score
- The score is then used to sort the members
- Use Cases
	- Leaderboards
	- Recommendation Engines

### Commands to work with sorted sets
`ZADD product:rank 4.5 apple`
`ZSCORE product:rank apple` - return the score
`ZRANK product:rank BOWTIE` - return a rank, ascending order, 0-based
`ZRANGE product:rank 0 -1 WITHSCORES` - return a range
`ZRANGE product:rank 4 5 BYSCORE WITHSCORES` - return item with score between 4 and 5 (inclusive)


## Hash
- Field and value, both string values
- Use Case
	- Store session data
	- Store records
		- We can do Index and Search on Hash
	- Cache records of relational db

### Commands to work with hash
`HSET product:bowtie42 name "Awesome Bowtie"`
`HGETALL product:bowtie42` - get all the fields and values


## JSON
- We can store serialized JSON or JSON document
- JSON document is easier to query specific field and allow typing
- Use Cases
	- Nested data (not supported by hash)
	- To store & cache records/session data
	- Allow index and search

### Commands to work with JSON
`JSON.SET product:apple $ '{"name":"Fuji", "color":"red"}'`
`JSON.GET product:apple`
`JSON.GET product:apple $.name`
`JSON.SET product:apple $.quantity 10`
`JSON.DEL product:apple $.quantity`


## Others
### Probabilistic data structures
- Sacrifices accuracy to gain speed
- Hyperloglog - counts practically unlimited number of unique items (with small error)
- Bloom filter - checks for membership (might give false positive)

### Streams
- Order data structure recording series of chronological events and their associated data

### Geospatial Index
- Longitude and latitude to collection of location 

### Vectors
- Since strings are binary-safe, we can do vector search on them
- We can do RAG using hashes

# References
[[Build With Lists]]
[[Build with Sets]]
[[Build with sorted sets]]
[[Build with Hash]]
[[Build with JSON]]
[[Other Data Structures]]