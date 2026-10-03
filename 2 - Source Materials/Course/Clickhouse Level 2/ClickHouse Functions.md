2025-11-22 12:13

# ClickHouse Functions
- There are 4 categories of ClickHouse function
- Regular - apply to each row separately like `lower`
- Aggregate - compute based on multiple rows like `quantile`
- Table - for creating table like `url`
- Window - window functions like standard SQL

We can check using `SELECT * system.functions`

## Regular Functions
- apply to each row separately like `lower`
	- Arithmetic functions
	- Date and time functions
	- Array functions
	- String functions - fuzzy match, haystack search string

## Aggregate Function
- Statistical function
- Exact vs approximation - for example `quantile` and `quantileExact`, `uniq`and `uniqExact` - the one without exact is approximation (faster)
- Count most frequent - `topK`
- Aggregate function combinators
	- Eg. *If* like`sumIf` which sum based on defined condition
- `any` functions used to include columns that are not in `group by` clause
- `arg` find not any value but specific condition like maximum `argMax`
- return the street that is most expensive in the town`SELECT town, argMax(street,price) FROM uk_price_paid GROUP BY town`

## User defined functions
- `CREATE FUNCTION mergePostcode AS (p1, p2) -> concat(p1, p2)`

# References
