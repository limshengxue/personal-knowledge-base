2025-06-22 19:55

# 1 - SQL
- SQL if master properly, help reduce the size of program and time to develop a features

## A First Use Case
In pgsql, data streaming can be achieved using
```sql
\copy factbook from 'factbook.csv' with delimiter E'\t' null ''
```
- Stream the file into the table `factbook`
- Delimiter set to tab
- Treat empty string as null

The data stream can be transform using code,
```sql
alter trades
	type bigint  
	using replace(trades, ',', '')::bigint
```
parse the column "trades" as `bigint` replace the thousand comma

In pgsql, we can declare and use variable like
```sql
\set start '2017-02-01'

select date, 
	to_char(shares, '99G999G999G999') as shares,
	to_char(trades, '99G999G999') as trades,
	to_char(dollars, 'L99G999G999G999') as dollars 
from factbook 
	where date >= date :'start' 
	and date < date :'start' + interval '1 month'
	order by date;
```

To get all the result with all the calendar day in February (show 0 when there is no record). We might use a loop in Python. However, we can also use pgsql `make_series` function.

```sql
select cast(calendar.entry as date) as date,
	coalesce(shares, 0) as shares, 
	coalesce(trades, 0) as trades, 
	to_char(
		coalesce(dollars, 0),
	'L99G999G999G999') as dollars 
from 
generate_series(
	date :'start',
	date :'start' + interval '1 month' - interval '1 day', 
	interval '1 day') as calendar(entry)  
left join factbook 
on factbook.date = calendar.entry  
order by date;
```
- `coalesce` return the first value if not null
- `generate_series` takes in the start and end value (inclusive), with the interval
- `calendar.entry` need to be cast from `timestamp` to `date`

To get a "week of week" changes
```sql
with computed_data as 
(
	select cast(date as date) as date, 
		to_char(date, 'Dy') as day, 
		coalesce(dollars, 0) as dollars, 
		lag(dollars, 1) 
			over( 
				partition by extract('isodow' from date) 
				order by date
		)  as last_week_dollars from 
		generate_series(date :'start'-interval '1 week', 
		date :'start' + interval '1 month' -interval '1 day', 
		interval '1 day' 
		 )  as calendar(date) 
		  left join factbook using(date) 
 ) 
select date, day, 
	to_char(
		coalesce(dollars, 0), 
		'L99G999G999G999') as dollars,
	case when dollars is not null
	and dollars <> 0 
	then round( 
			100.0 * (dollars - last_week_dollars)
			 / dollars , 
		 2)
		end  as "WoW %"
	from computed_data 
	where date >= date :'start'
order by date;
```
- The expression `extract(‘isodow’ from date)` allows getting the "day of week"
- The most important function here is the `LAG` function which is a *window function*
- A *window function* executes last in a query, right after `JOIN` and `WHERE`

### Writing Advanced SQL
- It is required by correctness for some processing to happened in SQL
- Help optimize performance by
	- Reduce round trip and latency
	- Reduce memory and bandwidth by reducing the size of the result size

## SQL Injection
- Happen when database server mistakenly consider a *dynamic argument* of a query as part of the *query text*
- PgSQL implements a protocol level facility to send static SQL query text separately from its dynamic arguments to prevent SQL injection
- **Never build a query string by concatenating query arguments directly into your query strings**

### Prepared Statement
- PgSQL provide a facility to send query string and arguments separately on the wire
```sql
prepare foo as
	select date, shares, trades, dollars 
	from factbook 
		where date >= $1::date 
		and date < $1::date + interval '1 month'
	order by date
```
Then execute the statement using
```sql
execute foo('2010-02-01');
```
An example driver that used prepared statement is `asyncpg`
```sql
pgconn = await asyncpg.connect(CONNSTRING)
stmt = await pgconn.prepare(sql) 
res = {}

for (date, shares, trades, dollars) in await stmt.fetch(date):
	res[date] = (shares, trades, dollars)
	
await pgconn.close()
```

# References
