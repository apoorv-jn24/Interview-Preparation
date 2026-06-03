# Views

[Back to Master README](../README.md)

## Table of Contents

- [Views](#views)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## Views

### What is a View?

A view is a **virtual table** defined by a stored `SELECT` query. It does not physically store data - it is re-executed every time it is queried. Views provide:
- **Simplification** - hide complex joins/aggregations behind a simple name
- **Security** - expose only specific columns/rows to certain users
- **Reusability** - define a complex query once, reference it many times

### Syntax

```sql
-- Create a view
CREATE VIEW view_name (col1, col2, ...) AS
SELECT ...
FROM ...
WHERE ...;

-- Query a view (just like a table)
SELECT * FROM view_name;

-- Drop a view
DROP VIEW view_name;
```

> **Note:** Views cannot contain `ORDER BY` clauses in their definition in standard SQL.

### Example

```sql
-- Create a view joining PERSONS and JOBS
CREATE VIEW PTV (PERSON, NAME, TITLE) AS
SELECT PERSON, NAME, TITLE
FROM PERSONS AS P, JOBS AS J
WHERE P.JOB = J.JOB;

-- Now query it like a table
SELECT * FROM PTV WHERE NAME LIKE 'E%';

-- Create a view for a sales summary
CREATE VIEW dept_salary_summary AS
SELECT
    department,
    COUNT(*) AS headcount,
    AVG(salary) AS avg_salary,
    SUM(salary) AS total_payroll
FROM employees
GROUP BY department;

-- Use the view
SELECT * FROM dept_salary_summary WHERE avg_salary > 70000;
```

### Updatable Views

A view can sometimes be used for INSERT/UPDATE/DELETE operations if:
- It is based on a **single table** (no joins)
- It includes **all NOT NULL columns** without defaults
- It has no `GROUP BY`, `DISTINCT`, or aggregate functions

```sql
-- Updatable view example
CREATE VIEW active_employees AS
SELECT * FROM employees WHERE status = 'active';

UPDATE active_employees SET salary = 80000 WHERE id = 5;
-- This updates the underlying EMPLOYEES table
```

### Views vs Tables vs CTEs

| Feature              | Table       | View             | CTE                |
|----------------------|-------------|------------------|--------------------|
| Stores data          |  Yes       |  No (virtual)   |  No (temporary)   |
| Reusable             |  Yes       |  Yes            |  Within query only|
| Can be indexed       |  Yes       |  Usually no     |  No               |
| Performance          | Base        | Re-executes query| Same as subquery   |
| Security/access ctrl |  Yes       |  Yes            |  No               |

#### Key Points to Remember

> - Views are **virtual** - they don't store data.
> - Dropping a view does **not** delete the underlying table data.
> - Views defined with joins, GROUP BY, or DISTINCT are typically **not updatable**.
> - `CREATE OR REPLACE VIEW` updates a view definition without dropping it first.
> - Useful for **row-level security**: create a view with a WHERE clause to expose only certain rows.

---

## Interview Tips

- A view stores a reusable query definition, not necessarily the data.
- Know when views can and cannot be updated.
- Use views to simplify access and hide complexity.

## LeetCode Patterns

- LeetCode usually uses inline queries instead of views.
- Think of a view as a saved subquery when explaining complex reporting logic.
- Use view-like CTEs to make multi-step solutions readable.
