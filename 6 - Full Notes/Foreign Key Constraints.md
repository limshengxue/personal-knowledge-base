2026-10-27

Tags: [[3 - Tags/database|database]] [[3 - Tags/schema|schema]]

# Foreign Key Constraints

## Core Idea
A foreign key protects a relationship: a referenced identifier must identify an allowed parent record.

## Example: Orders and Customers
An order with customer_id = 42 should not point to a customer that does not exist. A foreign key makes the database enforce this relationship for API calls, background jobs, and direct SQL alike.

```sql
CREATE TABLE customers (
    id bigint PRIMARY KEY
);
CREATE TABLE orders (
    id bigint PRIMARY KEY,
    customer_id bigint NOT NULL REFERENCES customers(id)
);
```

The NOT NULL requirement is separate: a nullable foreign-key column can represent an absent relationship.

## Business Decisions Still Matter
What should happen when a customer is removed?
- Reject removal while orders still refer to the customer.
- Keep the customer as an inactive/anonymized entity if retention rules require the order history.
- Cascade deletion only when deleting dependent rows matches the actual business meaning.

Do not choose CASCADE merely to silence an error. A valid reference also does not prove that the customer is eligible for a purchase or belongs to the correct tenant.

## Existing Data and Performance
Before adding the constraint, identify orphan rows and decide whether each should be corrected, retained through a legitimate parent, or moved under an approved archival policy. Do not invent parent data solely to make validation pass.

PostgreSQL does not automatically index the referencing columns. Assess joins and parent-row changes using [[PostgreSQL Indexing Strategy]]. An index improves access; the foreign key enforces the relationship.

See [[Unique Constraints]] for duplicate prevention and [[Database Migrations]] for safely introducing rules.

# References
[[2 - Source Materials/Course/Backend Fundamentals 03 - Query Plans Constraints and Migrations|Backend Fundamentals 03 - Query Plans Constraints and Migrations]]
- [PostgreSQL constraints](https://www.postgresql.org/docs/current/ddl-constraints.html)
