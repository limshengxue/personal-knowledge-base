2026-04-05 19:42

# 3 - Storage and Retrieval
- How can we store the data such that we can find it again when we ask for it?

## Data Structures That Power the Database
- Imagine the most simplest database, that allow insert key-value pairs
```bash
db_set () {
 echo "$1,$2" >> database 
} 

db_get () { 
grep "^$1," database | sc | tail -n 1 
}
```
- It simply append the key, value to the end of the file
- No overwrite behaviour, hence we need to get the last value with `tail`
- This is efficient for data writing (ignoring concurrency, storage reclaiming, error handling)
- Indeed many database is based on this log-based approach, which log refers to an append-only sequence of record
- However, data searching has a terrible performance `O(n)`
- We need index

### Hash Index
- Based on hash maps
- Example: keep an in-memory hash map where each key mapped to a byte offset in the data file (the location where the data can be found)
- If we have large number of writes per key, it is feasible to keep the whole thing in memory
- What if the log file is full: we can break down to segment and perform compaction (removing duplicated key in same segment) or merging of segments
- We never edit file, we create a new version of them, thus these process can be done in background while the old file continue to serve
![[Attachments/Pasted image 20260405195451.png]]
- Each segment can then has its own in-memory hash table

#### Implementation Details
- File format
	- Binary format > String > CSV
- Deleting record
	- Append a special record to the data file known as *tombstone* 
	- This is to maintain append-only behaviour (can benefit sync between multiple instances)
- Crash recovery
	- We can backup snapshot of hash index to prevent loss the hash index as they are in-memory
- Partially written record
	- Can ignore those record 
- Concurrency control
	- Easiest way is to allow only 1 writer thread

#### Benefits of Never Edit
- Append is much faster than random write, especially on magnetic spinning-disk hard drives
- Concurrency and crash recovery is simpler
	- Easy to sync
	- No mix of new and old data
	- Partially append data is easier to detect and ignored

#### Limitations
- Hash Table must fit in memory
- Range queries not efficient






# References
