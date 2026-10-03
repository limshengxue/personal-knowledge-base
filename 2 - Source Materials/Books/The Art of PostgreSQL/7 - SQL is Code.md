2025-06-30 19:56

# 7 - SQL is Code
- Any SQL statement embed some logic
- SQL is actually application code
- We need to apply methodology to ensure its quality just like we treat code
- principle of least astonishment (minimise surprise)

## SQL Style Guidelines
- Use line break to break down different SQL clauses
- We don't have to use ALLCAPS for the clauses with syntax highlight so common nowadays
- Avoid using number in `ORDER BY` (they can point to the column in `SELECT` clause with their order but make it harder to refactor)
- `Natural join` automatically select the join columns based on common column name
	- We should avoid using it as we can have same column name for different content under different table
- Keep alias naming meaningful

## Comments
- Give details on unusual or difficult to write
- Avoid having to second-guess the intentions of the author


# References
