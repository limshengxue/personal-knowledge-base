2026-10-03 15:43

Tags: [[3 - Tags/database|database]] [[3 - Tags/sql|sql]] [[3 - Tags/concurrency|concurrency]]

# PostgreSQL Transactions and Isolation Levels

## Core Idea
- A transaction puts operations within one commit or rollback boundary; it is not a mutex covering every concurrent request.
- Atomicity prevents partial execution. Consistency requires the resulting state to preserve constraints and business invariants.
- Isolation controls concurrent interaction. Durability concerns retaining committed results under the configured guarantees.
- The last-item example in [[Race Conditions]] shows why using transactions alone can still allow overselling.

## Database Transactions
A transaction treats a group of database operations as one unit of work that commits or rolls back together.

A purchase includes decrementing stock and creating an order. If the decrement succeeds but order creation fails, the database should not retain the half-finished state of reduced stock without an order.

- BEGIN: Start the transaction.
- COMMIT: Commit the transaction.
- ROLLBACK: Undo the transaction's uncommitted changes.

### Transaction Boundaries
A database transaction does not mean that only one request can execute the surrounding code. Isolation levels, query design, and locking determine concurrent behavior.

Database transactions also generally do not cover external systems:
- A database rollback does not retract an email that has already been sent.
- A database rollback does not automatically refund a payment already charged by an external provider.

External operations require additional idempotency, messaging, or compensation mechanisms. Database rollback is not a universal undo button.

## ACID Properties
ACID describes properties that correct, reliable transactions should satisfy. It should not be treated as four automatic, all-purpose features supplied entirely by the database.

### A — Atomicity
All database operations in the transaction take effect together or are rolled back together.

It asks: **Did only part of the operation take effect?**

“Atomic” here does not mean that execution can never interleave with another transaction.

### C — Consistency
A transaction should move the data from a state that satisfies constraints and business invariants to another state that satisfies those rules.

It asks: **Is the resulting state still valid?**

#### Why Is This a Separate Property?
Suppose accounts A and B each contain 100, for a total of 200. Incorrect transfer code:
1. Debits 20 from A.
2. Credits 30 to B.

The database can commit both operations together, satisfying atomicity. However, the total becomes 210, violating the invariant that this transfer must not create or destroy funds.

**Complete execution of incorrect logic is still incorrect.** This distinguishes A from C.

#### Who Is Responsible for C?
- The database enforces declared constraints, such as CHECK, UNIQUE, and FOREIGN KEY.
- The application must express correct business logic and use concurrency controls that preserve its business rules.
- The database does not automatically understand every rule in a requirements document or detect that a credit should have been 20 instead of 30.

For example, -50 is a valid numeric value. A declared CHECK (balance >= 0) constraint rejects an update to that balance. Without such a constraint, a transaction does not somehow infer that negative balances are forbidden.

Even CHECK (stock >= 0) does not prevent the overselling example from ending with stock = 0 and two orders. One valid field does not establish that the complete business state is valid.

ACID consistency concerns integrity and business invariants. **It is not replica synchronization or eventual consistency in distributed systems.**

### I — Isolation
Isolation defines what concurrent transactions can observe and how they may affect one another.

It asks: **How can another operation running concurrently affect mine?**

Isolation has different strengths. Starting a transaction does not automatically select the strongest level.

### D — Durability
Once a transaction is acknowledged as committed, its result should remain after recovery from a failure.

It asks: **Can an acknowledged result be lost?**

This guarantee depends on correct durability settings and reliable storage. It does not promise recovery after all storage is destroyed, and it does not replace backups.

## Isolation Levels
Isolation levels determine which concurrent changes transactions can observe and which anomalies the database prevents. The implementation details below refer to PostgreSQL.

### Read Uncommitted
The minimum guarantees associated with this level allow reading another transaction's uncommitted changes, which may later be rolled back. This is a dirty read.

PostgreSQL implements this level as Read Committed, so selecting it does not enable dirty reads.

### Read Committed
This is PostgreSQL's default isolation level. An ordinary query uses a statement-level snapshot: it sees data committed before that query began, as well as the transaction's own earlier changes.

For example, within one transaction:
1. The first stock query returns 10.
2. Another transaction changes stock to 9 and commits.
3. A second stock query may return 9.

Reading the same row again and observing a changed value is a nonrepeatable read.

Reading committed data does not make that data permanently current or guarantee that a later decision based on it remains valid.

### Repeatable Read
PostgreSQL uses a stable transaction snapshot established by the first non-transaction-control statement, not necessarily by BEGIN itself. A transaction can still see its own changes.

In the example above, both ordinary queries see 10 even if another transaction has committed 9.

PostgreSQL's Repeatable Read also prevents phantom reads: concurrent commits do not change the matching row set observed by repeated queries with the same condition. It still does not provide all Serializable guarantees.

#### Write Skew: A Stable Snapshot Does Not Guarantee Jointly Valid Decisions
Business rule: At least one of doctors A and B must remain on call.

1. A's transaction sees B on call and marks A off duty.
2. B's transaction sees A on call and marks B off duty.
3. The transactions modify different rows and may both commit.
4. Nobody remains on call, violating the rule.

This is write skew. Each decision looks reasonable within its own stable view, but the combined result is invalid.

### Serializable
Successful transactions must have an overall effect equivalent to some serial ordering of those transactions.

This does not physically limit all transaction execution to one at a time. PostgreSQL can execute transactions concurrently but may abort a transaction when conflicts make a safe outcome impossible. The application must retry the entire transaction.

#### Does It Solve the Last-Item Race?
Yes, provided the read, stock check, decrement, and order insertion are all inside the same Serializable transaction, and competing operations use appropriate concurrency controls too.

One possible sequence:
1. A and B both read stock = 1.
2. A decrements stock, creates an order, and commits.
3. B encounters a serialization failure and aborts.
4. The application retries B's transaction from the beginning and reads stock = 0.
5. B reports sold out instead of creating an order.

**Serializable does not necessarily prevent both transactions from initially reading 1. It prevents both purchases from committing unchanged when their combined outcome could not arise from a valid serial execution.**

#### Retry Boundaries
- Retry the entire transaction, including its reads and business decisions, not only the last failed SQL statement.
- Do not check stock outside the transaction and carry that stale decision into it.
- Bound retries and handle eventual failure; do not retry forever or pretend the operation succeeded.
- Do not repeat external charges, emails, or other side effects without appropriate safeguards.
- Serializable does not correct a transaction's own faulty logic, such as debiting 20 and crediting 30.

## Example
For a report that must keep a stable snapshot across multiple queries:

```sql
BEGIN ISOLATION LEVEL REPEATABLE READ READ ONLY;
SELECT albumid, title
FROM album
WHERE artistid = 1;

SELECT albumid, SUM(milliseconds) AS duration_ms
FROM track
GROUP BY albumid;
COMMIT;
```


## Correctness Decisions
- At read committed, two queries can observe different committed states even inside the same transaction.
- A single set-based query can avoid some cross-query inconsistencies, but it does not replace all concurrency controls.
- Snapshot stability alone does not enforce every cross-row business invariant.
- Keep transactions short and avoid waiting for user input while holding them open.
- Retry the entire transaction after a serialization failure, with bounded retries and safe handling of external side effects.
- PostgreSQL Repeatable Read can also produce serialization failures; retry handling is not exclusive to Serializable.

## Related Concepts
- [[Race Conditions]]: Fixed-value writes and conditional updates in the inventory example.
- [[Deadlocks]]: Transactions waiting for locks held by one another.
- [[SQL vs Application Business Logic]]: Choose the boundary between database and application rules.
- [[psql Workflows]]: Understand interactive transaction state.

# References
[[2 - Source Materials/Course/Backend Fundamentals 01 - Transactions and Concurrency|Backend Fundamentals 01 - Transactions and Concurrency]]
[[2 - Source Materials/Books/The Art of PostgreSQL/2 - Software Architecture|2 - Software Architecture]]
[[2 - Source Materials/Books/The Art of PostgreSQL/4 - Business Logic|4 - Business Logic]]
[[2 - Source Materials/Books/The Art of PostgreSQL/6 - Psql|6 - Psql]]
- [PostgreSQL Transactions](https://www.postgresql.org/docs/current/tutorial-transactions.html)
- [PostgreSQL Isolation](https://www.postgresql.org/docs/current/transaction-iso.html)
- [PostgreSQL Glossary](https://www.postgresql.org/docs/current/glossary.html)
