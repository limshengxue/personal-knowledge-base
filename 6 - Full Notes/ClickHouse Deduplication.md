2025-11-23 10:38

Tags: [[clickhouse]] [[database]] [[olap]]

# ClickHouse Deduplication
- Re-insert or update is common
	- When upsert data to overwrite old value
	- When client not sure if insert worked, so it re-sends
	- When have to update data frequently
- There are table engines designed for deleting duplicate records [[ClickHouse Table Engine and Parts]]

## Replacing Merge Tree
- More for upsert
- Remove record with duplicating sort key
	- When we insert a row with existing sort key, the new row will replace the existing row
- Actual deduplication occur during a merge
- If we want to view immediate result, do `SELECT * FROM foo FINAL;`. Adding the `FINAL` keyword add deduplication logic at query time, allow immediate result even if actual deduplication haven't occurred.
- We can specify `ver` column to decide which row to take. Else, it will use the *last row inserted* which can cause issue in multi-thread environment.

## Collapsing Merge Tree
- Can significantly reduce storage
	- and increase efficiency of `SELECT`
- It deletes pairs of rows if all of the columns in sort order are equivalent
- and if the two matching rows have different states (1 and -1)
- To update a row, we must delete a row first
![[Attachments/Pasted image 20251123092811.png]]
- Similarly, actual collapse occur during merge time
- We add `FINAL` to trigger collapse during query time
- The row with sign 1 is called *state row* while -1 is *cancel row*
- When we have more state rows than cancel row, it try best effort to return the latest value
- When there are at least 2 more state rows than cancel row or at least 2 more cancel row than state row (the rows cannot cancel out each other), the merge continue but error will be logged


### VersionedCollapsingMergeTree
- Add a `version` column to handle multi-thread environment


# References
[[2 - Source Materials/Course/Clickhouse Level 2/ClickHouse Deduplication|ClickHouse Deduplication]]
