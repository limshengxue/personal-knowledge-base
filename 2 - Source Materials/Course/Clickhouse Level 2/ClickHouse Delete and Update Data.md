2025-11-22 13:22

# ClickHouse Delete and Update Data
- ClickHouse parts are immutable files
	- Rewrite of files is required for delete and update
	- Thus, Delete and Update does not happen immediately
- Cannot update *primary key column*

## Mutations Commands
`ALTER TABLE random DELETE WHERE y != 'hello'`
- A heavy-weight event
- Return immediately but does not occur immediately
- Can check status of the event in `system.mutations`
- Execute in order which they were created
- Data inserted after a mutation is created not mutated
- If mutation get stuck, we can `KILL MUTATIONS`
- Handle through zookeeper for replicated tables

## Lightweight Deletes
`DELETE FROM my_table WHERE y != 'hello'`
- Use different syntax than a mutations
- Use marker columns to mark row as deleted
- Not actually deleted from file system
- Actual delete occur during next merge
- `SELECT` queries automatically rewritten to exclude the deleted rows
- Frequent lightweight delete have negative impact on performance


## On the fly Updates
Append `SET apply_mutations_on_fly = 1`  to alter table query
- Change value at query time
- `SELECT` queries get updated value immediately
- Actual update occur during next merge
- Frequent lightweight updates have negative impact on performance

## Lightweight Updates
Append `SET apply_mutations_on_fly = 1`  to alter table query
- Experimental features
- Write a patched update

# References
