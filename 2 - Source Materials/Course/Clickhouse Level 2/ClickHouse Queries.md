2025-11-22 11:23

# ClickHouse Queries
## Select Queries
- Most select syntax in SQL works
- But
	- We need to ask different questions that we do with an OLTP
	- We should take advantage of the smart functions in ClickHouse (there are over 1500 custom functions)

## Format Output
- We can control the output format
```SQL
SELECT name, age FROM users FORMAT TabSeparated;
```

## CTE
- Common Table Expression can be used for identifier or result set
- ![[Attachments/Pasted image 20251122112705.png]]


# References
