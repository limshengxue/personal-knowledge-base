2026-10-03 15:43

Tags: [[database]] [[sql]]

# psql Workflows
- `psql` is PostgreSQL's interactive terminal and scripting client.
- Backslash commands run in the client; SQL statements are sent to the database.
- Keep connection credentials out of command history and checked-in scripts.

## Connect and Inspect
```bash
psql -d "host=localhost port=5432 dbname=example user=reader"
```

Inside `psql`, use `\conninfo` for connection details, `\dt` for tables, `\d factbook` for a table description, and `\q` to exit.

## Variables and File Import
```sql
\set start '2017-02-01'
SELECT date, shares
FROM factbook
WHERE date >= :'start'::date
ORDER BY date;

\copy factbook FROM 'factbook.tsv' WITH (FORMAT text, DELIMITER E'\t', NULL '')
```

- `:'start'` inserts a quoted SQL literal; it is not driver parameter binding.
- `\copy` reads a file available to the client, unlike server-side file `COPY`.
- Confirm table layout and delimiters before loading data.

## Transaction Awareness
- `\set PROMPT1 '%~%x%# '` shows database name and transaction state.
- `%x` shows `*` for an open transaction and `!` for a failed one.
- `\set ON_ERROR_ROLLBACK interactive` uses savepoints to recover from statement errors interactively; it does not silently make every failed operation succeed.
- For scripts, stop on errors using `ON_ERROR_STOP`.

## Repeatable Reporting
```bash
psql -X -v ON_ERROR_STOP=1 -d example -f report.sql
psql -X -v ON_ERROR_STOP=1 -P format=html -d example -f report.sql > report.html
```

`-X` ignores startup customisation. Personal defaults can live in a psql startup file; verify the location for the operating system. [psql reference](https://www.postgresql.org/docs/current/app-psql.html).

# References
[[2 - Source Materials/Books/The Art of PostgreSQL/1 - SQL|1 - SQL]]
[[2 - Source Materials/Books/The Art of PostgreSQL/6 - Psql|6 - Psql]]

