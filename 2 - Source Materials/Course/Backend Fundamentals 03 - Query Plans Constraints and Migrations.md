2026-10-26 21:00

# Backend Fundamentals 03 - Query Plans Constraints and Migrations

## Headline
Measure expensive database work, enforce data relationships and uniqueness centrally, and evolve the schema through reviewed migrations.

## Summary
Lesson 3 covers concepts 11–15. The examples distinguish performance tools from correctness rules: a fast query can still return invalid data, while a correct constraint does not make a query fast. Follow-up discussion adds how to clean existing violations and how Alembic migration branches interact with GitLab/GitHub development.

This is an English synthesis of the technical lesson and follow-up, not a private conversation transcript. Business examples are illustrative, not reported production incidents.

## 11. Query Optimization
- Start with the slow business operation, such as a customer's recent-order list.
- Measure query count, elapsed time, returned rows, and waiting before changing SQL.
- Reduce unnecessary work: filter and paginate appropriately, avoid excess columns, and remove repeated related-record queries.
- Inspect the remaining plan and existing indexes rather than automatically adding indexes or enlarging the connection pool.
- Verify unchanged results, including duplicates, NULL behavior, and ordering.
- A JOIN can solve a round-trip problem while creating row multiplication if used carelessly. See [[Query Optimization]].

## 12. EXPLAIN ANALYZE
- EXPLAIN exposes a planned execution strategy; ANALYZE runs the statement and adds observations.
- Compare expected and actual rows, repeated loops, time, and buffer activity to locate work worth investigating.
- Planner cost is not elapsed milliseconds; node timings are not independent values to add indiscriminately.
- Run safely: a write statement really writes, and functions/triggers can have effects.
- One plan from a tiny test dataset does not establish production behavior. See [[EXPLAIN ANALYZE]].

## 13. Foreign Key Constraints
- An order's customer reference should point to a valid customer.
- Enforce this in the database instead of relying solely on every application path remembering the check.
- A relationship constraint and a mandatory-value rule are separate.
- Deletion behavior is a business decision: cascading away order history may be wrong.
- Existing orphan rows need a deliberate resolution before enforcement. See [[Foreign Key Constraints]].

## 14. Unique Constraints
- “One coupon claim per user” requires uniqueness of the user/coupon pair, not each column separately.
- Two requests can pass an application precheck concurrently; let the database enforce the final rule and handle rejection.
- Decide NULL, history, active-record, and tenant scope explicitly.
- A constraint's key must represent the intended business rule, not merely a convenient column. See [[Unique Constraints]].

## 15. Database Migrations
- Store ordered, reviewed schema/data changes alongside application evolution.
- ORM models describe desired structure; migration code explains how existing installations reach it.
- Generated scripts require review for data conversion, renames, locks, and old/new application compatibility.
- Test upgrades from realistic prior states, not only empty databases.
- Downgrade scripts cannot replace backups or restore already discarded information. See [[Database Migrations]].

## Follow-up: Violations Before Adding a Constraint
- The proposed reasoning was to identify violations, confirm business rules, and then correct data with SQL or archive old records.
- The refined workflow preserves authoritative records, dependencies, and audit needs instead of deleting arbitrary duplicates.
- Copying rows to an archive leaves the original violation present. Moving rows has additional effects on references and historical queries.
- Close the race between cleanup and enforcement: concurrent writers must not silently recreate invalid rows.

## Follow-up: Migration Head Conflicts in GitLab and GitHub
- A clean Git merge can still leave two Alembic heads because independent revision files share a parent.
- Rebase moves Git commits; it does not automatically redesign the migration graph.
- Private, unshared revisions can sometimes be regenerated/reparented after updating main. Already-shared/applied history should be preserved.
- A merge revision reconciles graph dependencies, not contradictory schema operations.
- CI should check the intended head policy and relevant upgrade paths on the proposed merged state.
- Revalidate when main changes; coordinate deployment so migrations are not independently launched by every service worker.
- Never erase or stamp history simply to hide an error.

## Key Ideas
- [[Query Optimization]] and [[EXPLAIN ANALYZE]] make performance work evidence-based.
- [[Foreign Key Constraints]] and [[Unique Constraints]] encode different correctness rules.
- [[Database Migrations]] connects schema changes, existing data, and deployment order.
- [[Zero-Downtime Migrations]] extends this foundation to rolling deployments.

## Remarks
- This lesson was delivered on 2026-10-26 Malaysia time and saved together with lesson 4 on 2026-10-27.
- Follow-up answers are learning examples, not evidence of completed production migrations.
- Private message URLs and unrelated personal information are omitted.

Co-authored by ChatGPT

# References
- [PostgreSQL Using EXPLAIN](https://www.postgresql.org/docs/current/using-explain.html)
- [PostgreSQL EXPLAIN command](https://www.postgresql.org/docs/current/sql-explain.html)
- [PostgreSQL constraints](https://www.postgresql.org/docs/current/ddl-constraints.html)
- [Alembic autogeneration](https://alembic.sqlalchemy.org/en/latest/autogenerate.html)
- [Alembic branches](https://alembic.sqlalchemy.org/en/latest/branches.html)
- [GitLab merged results pipelines](https://docs.gitlab.com/ci/pipelines/merged_results_pipelines/)
