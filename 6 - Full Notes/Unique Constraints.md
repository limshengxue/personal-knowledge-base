2026-10-27

Tags: [[3 - Tags/database|database]] [[3 - Tags/schema|schema]] [[3 - Tags/concurrency|concurrency]]

# Unique Constraints

## Core Idea
Make the database reject duplicate values for the key that represents the business rule.

## Example: One Coupon Claim Per User
A claim belongs to a user and a coupon. The rule is not “each user appears once” or “each coupon appears once,” but “each user/coupon pair appears once.”

```sql
ALTER TABLE coupon_claims
ADD CONSTRAINT coupon_claims_user_coupon_key
UNIQUE (user_id, coupon_id);
```

An application check can give friendly feedback, but two requests can both see no existing claim before either inserts. The database constraint is the final guard. Handle the failure deliberately; do not assume a preliminary SELECT eliminates [[Race Conditions]].

## Clarify the Rule First
- Are both key fields mandatory? Use appropriate NOT NULL rules.
- PostgreSQL ordinarily treats NULL values as distinct for uniqueness, so a nullable field changes the meaning of the rule.
- Does history permit several attempts but only one active claim? That may require a different model or a conditional unique index rather than uniqueness across all history.
- Does uniqueness apply globally or within a tenant?

## Introducing the Rule Into Dirty Data
1. Find violations of the exact intended key.
2. Decide which record is authoritative using business rules, not arbitrary row order.
3. Correct or reconcile duplicates while preserving dependent references and required audit history.
4. If archiving is appropriate, distinguish copying rows from moving them: a copy does not remove violations in the constrained table.
5. Prevent new violations during the cleanup-to-enforcement gap, then validate and establish the constraint.

A reviewed migration can fail rather than silently retain corrupt data. Archive retention, access, and recovery requirements remain separate decisions. See [[Database Migrations]].

# References
[[2 - Source Materials/Course/Backend Fundamentals 03 - Query Plans Constraints and Migrations|Backend Fundamentals 03 - Query Plans Constraints and Migrations]]
- [PostgreSQL constraints](https://www.postgresql.org/docs/current/ddl-constraints.html)
