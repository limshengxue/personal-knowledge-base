2025-06-30 19:32

# 6 - Psql
- CLI tool 
- For scripting and interactive usage
- REPL - read, eval, print loop

## Setup 
- We can define some setup commands in `~/.psqlrc` file
- The useful one include

### Define Transaction
`\set PROMPT1 '%~%x%# '`
- It will set a `*` when there is a transaction ongoing
- And `!` when the transaction has error

### Define Error Rollback
`\set ON_ERROR_ROLLBACK interactive`
- Ignore error instead of forcing rollback in interactive session (but not during executing script)

## Reporting Tool
- Psql can also be used as a reporting tool other than interactive tool
- By setting the format argument, the result can be returned in HTML format
```
psql
	--tuples-only 
	--set n=1 
	--set name=Alesi 
	--no-psqlrc // no need to read the startup file
	-P format=html 
	-d f1db  // database name
	-f report.sql
```
- We can define the connection string with `-d`
```
psql
-d postgresql://dim@localhost:5432/f1db 

psql
-d "user=dim host=localhost port=5432 dbname=f1db"
```

# References
