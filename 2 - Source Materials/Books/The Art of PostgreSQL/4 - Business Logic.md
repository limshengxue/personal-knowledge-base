2025-06-23 19:50

# 4 - Business Logic
How much business logic should be maintained in the database
- Depends on *correctness* and *efficiency*

## Every SQL embed some business logic
 An obvious SQL that contains business logic is,
 ```sql
select name, 
	milliseconds * interval '1 ms' as duration,
	pg_size_pretty(bytes) as bytes
from track 
where albumid = 193
order by trackid;
```
because it shows some form of derivations from the raw values being stored.

However, a simple SQL like 
```sql
select name from track 
where albumid = 193 
order by trackid;
```
also embed some business logic
- The `select` clause state which column is relevant to the application
- The `where` strongly tied to the business logic on which record we are interested in
- `order by` encapsulate how we want to display the record

## Business Logic Applies to Use Case
- Consider a simple use case: display a list of albums from a given artist, each with its total duration
- Which can be implemented using the query:
```sql
select album.title as album, 
	sum(milliseconds) * interval '1 ms' as duration 
from album 
	join artist using(artistid) 
	left join track using(albumid)
where artist.name = 'Red Hot Chili Peppers' 
group by album 
order by album;
```
- If we would to migrate this business logic to application code we can perform
	- 1. Fetch list of albums for selected artist
	- 2. For each album, fetch the duration of every track in the album
	- 3. In the application, sum up the durations per album
- This is very inefficient, while we don't write such code, some object model API might do not on the background

## Correctness
- Correctness depends heavily on *isolation levels* of the db
- The SQL standard define four isolation levels and PostgreSQL implements three of them, leaving out dirty reads
- The isolation level determines which side effects from other transactions your transaction is sensitive to.
- **Read uncommited** - Pgsql implement read committed for this
- **Read committed** - ensure what have been read is committed by other transaction, cannot guarantee repeatable read in single transaction  (**default**)
- **Repeatable read** - transaction keep a snapshot of the whole database, from BEGIN to COMMIT
- **Serializable** - This level guarantees one-transaction-at-a-time (prevent race condition)
- Each transaction can have different isolation level
- When we have the *read committed* the application code implementation could went wrong
	- Between "fetch all album" and "fetch all track of an album", if a user delete an album, user can see album with zero duration

## Efficiency
- Can be measured statically or dynamically
	- Statically - time to write, review, maintain the code
	- Dynamic - what happened in runtime, time and resources to run the code
- The SQL approach took only a single round trip while the application took 1 (to fetch artist) + 1 (to fetch albums) + n (to fetch tracks for each album)

## Stored Procedures - Data Access API
- Server-side functions
- SQL objects that store code and execute when called
- Example:
```sql
create or replace function get_all_albums 
(
	in id BIGINT, 
	out album text, 
	out duration interval
)
returns setof record 
language sql 
as $$
	select album.title as album, 
		sum(milliseconds) * interval '1 ms' as duration
	from album
		join artist using(artistid)
		left join track using(albumid)
	where artist.id = get_all_albums.id
group by album 
order by album;
$$;
```
- If we want to search by name, we can perform lateral join
```sql
select album, duration 
	from artist, lateral get_all_albums(artistid) 
where artist.name = 'Red Hot Chili Peppers';
```


## When to use Stored Procedure
- We must know when to use procedural code or plain SQL with parameters
- A bad example
```sql
create or replace function get_all_albums 
(
	in name text, 
	out album text,
	out duration interval
)
returns setof record 
language plpgsql 

as $$ 
declare 
	rec record; 
begin 
	for rec in select albumid 
		from album 
		join artist using(artistid) 
		where album.name = get_all_albums.name
	loop 
		select title, sum(milliseconds) * interval '1ms'
		into album, duration 
		from album
			left join track using(albumid)
		 where albumid = record.albumid
	group by title 
	order by title; 
	return next;
	end loop; 
end; 
$$;
```

## Where to Implement Business Logic
- First solution - application code (incorrect, inefficient)
- Using stored procedure allow building data access API, maintain it in transactional way
- Another advantage of stored procedure is less data get send over the network (no need to send the whole query)

# References
