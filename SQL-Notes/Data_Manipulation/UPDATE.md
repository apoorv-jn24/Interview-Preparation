# UPDATE

[Back to Master README](../README.md)

## Table of Contents

- [UPDATE](#update)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## UPDATE

### Definition
Modifies existing rows in a table.

### Syntax

```sql
UPDATE table_name
SET col1 = val1, col2 = val2, ...
WHERE condition;
```

### WARNING
**Omitting the WHERE clause updates ALL rows!** Always double-check your WHERE condition before running UPDATE.

### Explanation
- Multiple columns can be updated in one statement using comma-separated `col = value` pairs.
- The `WHERE` clause restricts which rows are updated.
- Values can be expressions: `SET salary = salary * 1.10`.

### Example

```sql
-- Update specific columns for one row
UPDATE COUNTRIES
SET AREA = 53501998, LANGUAGE = 'Hebrew', POP = 100000000
WHERE COUNTRY = 'Beulah';

-- Give a 10% raise to all employees in IT
UPDATE employees
SET salary = salary * 1.10
WHERE department = 'IT';

-- Update using a subquery
UPDATE products
SET price = price * 0.90
WHERE category_id IN (
    SELECT id FROM categories WHERE name = 'Clearance'
);
```

---

## Interview Tips

- Always include a WHERE clause unless intentionally updating every row.
- Explain how transactions protect updates from partial failure.
- Know how constraints and triggers may affect updates.

## LeetCode Patterns

- LeetCode rarely uses UPDATE, but interview scenarios often test safe modification.
- Use CASE WHEN in UPDATE for conditional changes.
- Validate affected rows before committing in real systems.
