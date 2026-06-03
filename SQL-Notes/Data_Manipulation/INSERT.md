# INSERT

[Back to Master README](../README.md)

## Table of Contents

- [INSERT](#insert)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## INSERT

### Definition
Adds one or more new rows to a table.

### Syntax

```sql
-- Insert with column list (recommended)
INSERT INTO table_name (col1, col2, col3)
VALUES (val1, val2, val3);

-- Insert without column list (must supply all columns in order)
INSERT INTO table_name
VALUES (val1, val2, val3, ...);

-- Insert multiple rows
INSERT INTO table_name (col1, col2)
VALUES (v1, v2), (v3, v4), (v5, v6);

-- Insert from a SELECT
INSERT INTO table_name (col1, col2)
SELECT col1, col2 FROM other_table WHERE condition;
```

### Explanation
- Always provide the **column list** - it protects against schema changes and makes the query self-documenting.
- Unlisted columns are assigned their **default values** (or NULL if no default is set).
- Values must match the column types in order.

### Example

```sql
-- Before INSERT
-- COUNTRIES: Algeria, Yugoslavia

INSERT INTO COUNTRIES (COUNTRY, LITERACY, AREA, POP)
VALUES ('Beulah', 100, 1146807, 144000);

-- After INSERT
-- COUNTRIES: Algeria, Yugoslavia, Beulah
```

```sql
-- Modern example
INSERT INTO employees (first_name, last_name, email, hire_date, salary, dept_id)
VALUES ('Jane', 'Smith', 'jane@example.com', '2024-01-15', 75000.00, 3);

-- Insert from another table
INSERT INTO archived_employees (id, name, email, left_date)
SELECT id, name, email, CURRENT_DATE
FROM employees
WHERE termination_date IS NOT NULL;
```

### Key Points to Remember

> - Always specify the **column list** in `INSERT INTO`.
> - Missing columns get default values; if no default and NOT NULL constraint exists, an error is raised.
> - `INSERT ... SELECT` is powerful for bulk data operations.

---

## Interview Tips

- Always specify target columns unless inserting into every column in exact table order.
- Understand INSERT INTO ... SELECT for copying query results into a table.
- Mention defaults, identity columns, and constraint checks.

## LeetCode Patterns

- LeetCode SQL is mostly SELECT, but INSERT knowledge helps explain data setup.
- Use INSERT examples to reason about table rows and constraints.
- Understand how inserted NULL values interact with NOT NULL and DEFAULT constraints.
