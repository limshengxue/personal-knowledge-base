2026-10-27

Tags: [[3 - Tags/database|database]] [[3 - Tags/schema|schema]] [[3 - Tags/ci cd|ci cd]]

# Database Migrations

## Core Idea
Version and review database changes so environments can move through a known, reproducible sequence instead of relying on remembered manual SQL.

## A Migration Is More Than an ORM Model Edit
Adding a model field does not necessarily update an existing database. A migration states how to transform the current schema or data, and records where it belongs in history.

Typical workflow:
1. Inspect current schema and existing data.
2. Write or generate a candidate migration.
3. Review SQL, data rules, locking, compatibility, and failure handling.
4. Test from an empty database and from a representative previously deployed state.
5. Deploy through one controlled migration runner and verify the result.

Alembic autogeneration produces candidates, not proof of a safe migration. Renames and application-specific data transformations need particular scrutiny. A downgrade is not a backup: dropping a populated column can destroy information that recreating it cannot recover.

## Constraint Cleanup
Before adding [[Foreign Key Constraints]] or [[Unique Constraints]], inspect violations and confirm the business rule. Correcting or archiving rows requires explicit decisions about retained records, dependencies, and history. Prevent concurrent writers from recreating violations between cleanup and enforcement.

## Git Conflicts Versus Alembic Heads
Two developers can add separate files without a Git textual conflict while both migrations name the same parent revision. The revision graph now has two heads. Git rebase alone does not rewrite down_revision.

- A private, unapplied/unshared revision may be safely regenerated or reparented after synchronizing main, with local database state reconciled deliberately.
- Preserve published/applied history. A merge revision can join two heads after checking the operations are compatible.
- A merge revision joins dependency paths; it does not fix two branches both creating the same column or incompatible data assumptions.
- Use explicit revision IDs when resolving known heads. “heads” reports repository tips; “current” reports the database's recorded revision state.
- Do not delete history or use stamp merely to suppress an error; stamping does not execute missing changes.

## CI and Deployment Guardrails
Test the proposed merged result, not only the feature branch. A single-head assertion is appropriate only if the repository intentionally requires one head. Test relevant upgrade paths, including each existing branch state when merging revisions.

Use a merge queue/train or equivalent revalidation to reduce races between independently passing changes. Configure the platform features appropriately. Keep migration execution serialized for a deployment rather than having every application worker run it.

[[Zero-Downtime Migrations]] adds a deployment compatibility strategy across old and new application versions.

# References
[[2 - Source Materials/Course/Backend Fundamentals 03 - Query Plans Constraints and Migrations|Backend Fundamentals 03 - Query Plans Constraints and Migrations]]
- [Alembic branches](https://alembic.sqlalchemy.org/en/latest/branches.html)
- [Alembic autogeneration](https://alembic.sqlalchemy.org/en/latest/autogenerate.html)
- [GitLab merged results pipelines](https://docs.gitlab.com/ci/pipelines/merged_results_pipelines/)
