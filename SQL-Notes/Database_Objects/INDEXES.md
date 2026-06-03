# Indexes

[Back to Master README](../README.md)

## Table of Contents

- [Indexes](#indexes)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## Indexes

### What is an Index?

An index is an invisible data structure that speeds up data retrieval on a table. Think of it like a book's index - instead of scanning every page, you jump directly to the relevant location. Indexes are created with `CREATE INDEX` and used **automatically** by the SQL query engine when beneficial.

### Trade-offs

| Benefit                        | Cost                                  |
|--------------------------------|---------------------------------------|
| Faster SELECT queries          | Slower INSERT/UPDATE/DELETE (index must be updated) |
| Faster JOINs on indexed columns| Consumes additional disk space        |
| Faster ORDER BY / WHERE        | Overhead of maintaining index structure |

### Syntax

```sql
-- Create an index
CREATE INDEX index_name
ON table_name (column1, column2);

-- Create a unique index (also enforces uniqueness)
CREATE UNIQUE INDEX index_name
ON table_name (column);

-- Drop an index
DROP INDEX index_name;
```

### Example

```sql
-- Create an index on PERSONS to speed up queries by GENDER and JOB
CREATE INDEX GJX
ON PERSONS (GENDER, JOB);

-- Index on email for fast lookups
CREATE UNIQUE INDEX idx_user_email
ON users (email);

-- Composite index for a common query pattern
CREATE INDEX idx_orders_customer_date
ON orders (customer_id, order_date);

-- Remove index
DROP INDEX GJX;
```

### When to Create Indexes

 **Good candidates for indexing:**
- Columns frequently used in `WHERE` clauses
- Columns used in `JOIN` conditions (`ON table1.col = table2.col`)
- Columns used in `ORDER BY` or `GROUP BY`
- Columns with high cardinality (many unique values)
- Foreign key columns

 **Bad candidates:**
- Small tables (full scan is just as fast)
- Columns that are rarely queried
- Columns with very low cardinality (e.g., boolean columns)
- Tables with very frequent writes

#### Key Points to Remember

> - Indexes are **automatically used** by the query optimizer - you don't reference them in queries.
> - `PRIMARY KEY` columns are automatically indexed.
> - `UNIQUE` constraints automatically create an index.
> - Too many indexes slow down writes - index strategically, not exhaustively.
> - Use `EXPLAIN` / `EXPLAIN ANALYZE` to see whether an index is being used.

---

## Interview Tips

- An index speeds reads but adds write and storage overhead.
- Indexes are most useful on join keys, filter columns, and sort columns.
- Mention selectivity when explaining whether an index helps.

## LeetCode Patterns

- LeetCode focuses on correctness, but indexes matter in interview optimization discussions.
- Recognize columns that would benefit from indexes in large production tables.
- Avoid wrapping indexed columns in functions in filters.
