2025-06-22 20:46

# 2 - Software Architecture
Important concepts for advanced SQL
- RDBMS
- ACID - Atomic, Consistent, Isolated, Durable
	- Concept of Transaction (Atomic and Isolated)
	- Keep data consistent with business rules (pgsql support *constraint*)
	- Durable - backup of data (anything happened, committed changes won't lost unless disk corruption)
- Data Access API and Service
	- PgSQL can be think of a stateful data access service
	- Its API is SQL
- SQL is Declarative 
	- Developer describe what they want
	- It is the responsibility of pgsql to figure out how to do it in the most efficient way
- When designing software, don't think of pgsql as *storage layer* but rather a *concurrent data access service* which is capable of *data processing*

## The Extensible of PGSQL
- An example is Schemaless Design in PGSQL
```sql
select jsonb_pretty(data) 
from magic.cards 
where data @> '{ 
	"type":"Enchantment", 
	"artist":"Jim Murray", 
	"colors":["White"]
}';
```
- **`SELECT jsonb_pretty(data)`**:  
    Returns the `data` column in a pretty-printed (human-readable) JSON format.
- **`FROM magic.cards`**:  
    Pulls data from the `cards` table in the `magic` schema.
- **`WHERE data @> ...`**:  
    Filters the results to rows where the `data` (a `jsonb` column) contains:
    - `"type": "Enchantment"`
    - `"artist": "Jim Murray"`
    - `"colors": ["White"]`

# References
