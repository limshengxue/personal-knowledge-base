2026-10-03 15:43

Tags: [[database]] [[sql]]

# SQL Parameterization and Prepared Statements
- SQL injection occurs when untrusted values become part of SQL syntax instead of remaining data.
- Pass query text and parameter values separately through the database driver.
- Do not concatenate user input into a query string, even when the input appears numeric. [PostgreSQL parameter execution](https://www.postgresql.org/docs/current/libpq-exec.html).

## Parameterization vs Preparation
- Parameterization separates data from syntax; explicit preparation is not required to obtain this separation.
- A prepared statement stores a statement for later execution within a database session.
- Repeated execution may reuse planning work, but PostgreSQL can choose custom or generic plans. Preparation does not guarantee a faster plan. [PREPARE](https://www.postgresql.org/docs/current/sql-prepare.html).

## PostgreSQL Example
Assume `factbook(date, shares, trades, dollars)` exists:

```sql
PREPARE monthly_report(date) AS
SELECT date, shares, trades, dollars
FROM factbook
WHERE date >= $1
  AND date < $1 + INTERVAL '1 month'
ORDER BY date;

EXECUTE monthly_report(DATE '2017-02-01');
DEALLOCATE monthly_report;
```

## Application Boundary
- Use the driver's placeholders and binding API; placeholder syntax differs between drivers.
- Parameters represent values, not table names, column names, or arbitrary SQL fragments.
- For selectable sort columns, map input to a small allowlist of known identifiers rather than interpolating unchecked text.
- Avoid placing quotation marks around a driver placeholder unless its documentation requires them.

## psql Is Different
- `psql` variables are textual interpolation, not protocol-level parameters.
- `:'name'` quotes a value as a SQL literal; bare `:name` inserts text directly.
- Use [[psql Workflows]] for trusted reporting scripts and a driver's binding API for application requests.

# References
[[2 - Source Materials/Books/The Art of PostgreSQL/1 - SQL|1 - SQL]]

