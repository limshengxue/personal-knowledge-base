2026-10-03 15:43

Tags: [[database]] [[sql]]

# PostgreSQL JSONB Queries
- `jsonb` stores JSON in a processed representation suitable for querying and indexing.
- JSONB can represent flexible attributes alongside ordinary relational columns.
- Flexible structure does not remove the need to define required fields, types, and application contracts.

## Useful Operators
| Expression | Result |
| --- | --- |
| `data -> 'type'` | JSON value |
| `data ->> 'type'` | Text value |
| `data @> '{"type":"Enchantment"}'::jsonb` | Containment test |
| `jsonb_pretty(data)` | Readable text rendering |

Extraction of a missing field returns SQL null rather than an error. SQL null, a missing key, and a JSON null value are different cases. [JSON functions and operators](https://www.postgresql.org/docs/current/functions-json.html).

## Containment Example
Assume `magic.cards.data` is a JSONB column:

```sql
SELECT jsonb_pretty(data)
FROM magic.cards
WHERE data @> '{
  "type": "Enchantment",
  "artist": "Jim Murray",
  "colors": ["White"]
}'::jsonb;
```

- The predicate asks whether the stored document contains the specified structure; it is not a full-document equality test.
- Containment involving arrays is not a test that the entire stored array equals the supplied array.
- Use explicit casts when converting extracted text to dates or numbers, and validate input types.

## Modelling Trade-offs
- Keep stable identifiers and relational relationships in ordinary columns where practical.
- Use JSONB for attributes whose shape genuinely varies.
- Avoid fetching every document and filtering it in application memory when a database predicate expresses the requirement.
- Select an index supporting the actual operators and workload; see [[PostgreSQL Indexing Strategy]].
- Bind user-supplied values through [[SQL Parameterization and Prepared Statements]] rather than assembling JSON predicates through string concatenation.

# References
[[2 - Source Materials/Books/The Art of PostgreSQL/2 - Software Architecture|2 - Software Architecture]]

