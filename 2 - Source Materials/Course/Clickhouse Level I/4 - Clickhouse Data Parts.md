2025-11-09 10:22

# 4 - Clickhouse Data Parts
## Column Files
- Part folder contain column files 
- Which are immutable file that store the data (in column-oriented form)

## Primary Key
- Determine the sort order
- *Have nothing to do with uniqueness*
- Determine the columns used to build the primary index
- Therefore, `ORDER BY` and `PRIMARY KEY` is mostly equivalent

## Primary Index
- Each MergeTree table has a primary index
- The primary index consists of a key per granule
- Granule is the smallest amount of data that ClickHouse reads when searching row (8192 row)
- The key is not the unique value of the primary key, it is the value of the first row (its primary key) of the granule
- As the data is sorted by primary key, this info allow us to skip granule, for example, skipping granule 1 and granule 4 and any granule after granule 4

![[Attachments/Pasted image 20251109102829.png]]

## Granule Processing
- Each granule are sent to a thread for processing

## Primary Key Best Practices
- We can observe how many rows are read in the query in the SQL console
- Filter starting with the *first column provide optimal performance*
- We have to value the pros and cons of index
- Index bring cost of additional storage and processing during writing of data and merging of parts
- Primary key cannot be too huge, **it must fit in the memory**
- Use columns that are *frequently queried*
- If multiple primary key columns have equal importance, order by *cardinality* (allow skipping more granule)
- Primary Key also affect the *compression rate* - string give better compression

## Order By
- It is equivalent to primary key
- But it can also use to *extend* primary key if we want a different sort order
- For example, the z column below that exist in ORDER BY but not primary key, will not be in primary index
![[Attachments/Pasted image 20251109104429.png]]


## Additional Primary Indexes
- Create 2 tables for the same data
- Use projection
- Use a materialized view
	- Stores the data in a separated table with SELECT statement
	- Sort the data in the SELECT statement
- Define a skipping index

# References
