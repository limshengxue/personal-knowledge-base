2025-11-23 10:35

Tags: [[clickhouse]] [[database]] [[olap]]

# ClickHouse Delete and Update Data
- MergeTree data parts are immutable; updates use replacement data or patches rather than arbitrary in-place edits.
- Different mechanisms have different visibility, cleanup, and performance behavior.
- See [[ClickHouse Table Engine and Parts]] for the storage model.

## Heavyweight Mutations
```sql
ALTER TABLE random DELETE WHERE y != 'hello';
```
- Mutations rewrite affected data asynchronously unless configured to wait.
- Inspect `system.mutations` to track progress.
- Rows inserted after a mutation's relevant boundary are not automatically included.
- Use the documented `KILL MUTATION` statement when cancellation is appropriate.
- Replicated metadata coordination can use ClickHouse Keeper or a compatible legacy ZooKeeper deployment.

## Lightweight Deletes
```sql
DELETE FROM my_table WHERE y != 'hello';
```
- Marks rows so normal reads exclude them.
- Physical removal usually occurs during later merges; do not treat it as immediate secure erasure.
- Frequent deletes still impose storage and query costs.

## On the Fly Mutations
- `apply_mutations_on_fly` lets reads apply pending mutation expressions before background rewriting finishes.
- It is distinct from lightweight patch-part updates.
- Enable the setting in the relevant mutation and subsequent read contexts, rather than appending a second SET statement to an ALTER command.

```sql
SET apply_mutations_on_fly = 1;
ALTER TABLE my_table UPDATE y = 'updated' WHERE id = 1;
SELECT y FROM my_table WHERE id = 1;
```

The example assumes columns `id` and `y`. Background mutation processing still occurs; it is not simply deferred until the next ordinary merge.

## Lightweight Updates
- Supported releases provide SQL `UPDATE` using patch parts, not `apply_mutations_on_fly`.
- Check the installed release's availability, required settings, and table restrictions before using it.
- Patches impose read and merge overhead; choose a mechanism based on update size and frequency.
- Sorting/primary-key columns have update restrictions.

# References
[[2 - Source Materials/Course/Clickhouse Level 2/ClickHouse Delete and Update Data|ClickHouse Delete and Update Data]]
[Update mechanisms and tradeoffs](https://github.com/ClickHouse/clickhouse-docs/blob/main/docs/managing-data/updating-data/overview.mdx)
