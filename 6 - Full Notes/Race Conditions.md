2026-10-24 21:00

Tags: [[3 - Tags/concurrency|concurrency]] [[3 - Tags/database|database]]

# Race Conditions
A race condition occurs when correctness depends on how multiple operations interleave, and some possible orderings produce an incorrect result.

### Example: Selling the Last Item

Initial stock is 1. The application reads the stock, subtracts 1 in application code, then writes the calculated fixed value back.

| Step | Request A | Request B |
| --- | --- | --- |
| 1 | Reads stock = 1 | |
| 2 | | Reads stock = 1 |
| 3 | Writes stock = 0 and creates an order | |
| 4 | | Writes stock = 0 and creates an order |

The database now shows stock = 0, but there are two orders. Each request works correctly in isolation; their interleaving causes overselling.

The operations do not need to occur in the same nanosecond. **Another request can change the state between checking a condition and acting on it.**

### Unsafe Read-Then-Write

The following illustrates application/database interaction. It is not a complete, directly executable business workflow:

```sql
BEGIN;

SELECT stock FROM products WHERE id = 42;
-- The application reads 1, allows the purchase, and calculates new_stock = 0.

UPDATE products SET stock = 0 WHERE id = 42;
INSERT INTO orders (product_id, quantity) VALUES (42, 1);

COMMIT;
```

At PostgreSQL's default Read Committed level, both transactions can read 1 first. Even if the second update waits for the first transaction's row lock, it can subsequently write the fixed value 0 again. Both orders can commit.

**Adding BEGIN/COMMIT alone does not fix this logic.**

### One Fix: Conditional Update and Result Checking

Move the stock check and decrement into the same database update:

```sql
BEGIN;

UPDATE products
SET stock = stock - 1
WHERE id = 42 AND stock > 0
RETURNING id;

-- The application must inspect the result:
-- One returned row: execute the order insertion below.
-- Zero returned rows: stop, ROLLBACK, and report sold out or missing product.

INSERT INTO orders (product_id, quantity) VALUES (42, 1);

COMMIT;
```

The application implements this branch. Do not ignore the comments and execute INSERT unconditionally.

At Read Committed, after a competing update waits, PostgreSQL re-evaluates the WHERE condition against the updated target row. If stock is now 0, the condition no longer matches, so the second decrement does not succeed.

The decrement and order insertion still belong in one transaction: if order creation fails, roll back the decrement as well. This is a solution for single-product stock allocation, not a universal solution to every concurrency problem.

## Related Concepts
- [[PostgreSQL Transactions and Isolation Levels]]: Correct transaction boundaries and Serializable offer another way to handle the race above.
- [[Deadlocks]]: Locks can introduce circular waiting.
- [[Concurrency vs Parallelism]]: Interleaving is enough to create a race; true parallel execution is not required.

# References
[[2 - Source Materials/Course/Backend Fundamentals 01 - Transactions and Concurrency|Backend Fundamentals 01 - Transactions and Concurrency]]
- [PostgreSQL Read Committed](https://www.postgresql.org/docs/current/transaction-iso.html#XACT-READ-COMMITTED)
