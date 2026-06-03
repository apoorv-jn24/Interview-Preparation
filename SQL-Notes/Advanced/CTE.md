# CTE

[Back to Master README](../README.md)

## Table of Contents

- [CTEs (Common Table Expressions)](#ctes-common-table-expressions)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## CTEs (Common Table Expressions)

### What is a CTE?

A Common Table Expression (CTE) is a **named, temporary result set** defined at the beginning of a query using the `WITH` keyword. It exists only for the duration of the query and can be referenced like a table or view. CTEs improve readability and allow you to break complex queries into logical steps.

### Syntax

```sql
WITH cte_name AS (
    SELECT ...
    FROM ...
    WHERE ...
),
second_cte AS (
    SELECT ...
    FROM cte_name
    WHERE ...
)
SELECT *
FROM second_cte
WHERE ...;
```

### Non-Recursive CTE

```sql
-- Find employees earning above their department average
WITH dept_avg AS (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT e.name, e.salary, d.avg_salary
FROM employees e
JOIN dept_avg d ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary;
```

### Recursive CTE

A recursive CTE references itself and is used for hierarchical data (org charts, paths in a graph).

```sql
-- Traverse an employee hierarchy
WITH RECURSIVE org_chart AS (
    -- Anchor: start with top-level employees (no manager)
    SELECT id, name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive: find employees reporting to the previous level
    SELECT e.id, e.name, e.manager_id, oc.level + 1
    FROM employees e
    INNER JOIN org_chart oc ON e.manager_id = oc.id
)
SELECT * FROM org_chart ORDER BY level, name;
```

### Multiple CTEs

```sql
WITH
high_value_customers AS (
    SELECT customer_id FROM orders GROUP BY customer_id HAVING SUM(total) > 10000
),
recent_orders AS (
    SELECT * FROM orders WHERE order_date >= '2024-01-01'
)
SELECT c.name, r.order_id, r.total
FROM customers c
JOIN high_value_customers hvc ON c.id = hvc.customer_id
JOIN recent_orders r ON c.id = r.customer_id;
```

### CTE vs Subquery vs Temp Table

| Feature              | CTE                 | Subquery          | Temp Table         |
|----------------------|---------------------|-------------------|--------------------|
| Readability          |  High              |  Can be messy    |  High             |
| Reusability (same q) |  Yes               |  Must repeat     |  Yes              |
| Persists beyond query|  No                |  No             |  Yes (session)    |
| Recursive            |  Yes               |  No             |  No               |
| Indexes              |  No                |  No             |  Yes              |
| Performance          | Same as subquery    | Same as CTE       | Better for reuse   |

#### Key Points to Remember

> - CTEs exist only for the **duration of the query** - they are not stored.
> - Multiple CTEs are separated by commas; only one `WITH` keyword is used.
> - A CTE can reference a previously defined CTE in the same `WITH` block.
> - Recursive CTEs require `UNION ALL` (not `UNION`) between anchor and recursive parts.
> - CTEs are **not** always more performant than subqueries - the optimizer usually treats them the same.

#### Common LeetCode Pattern

```sql
-- LC 1164: Product Price at a Given Date
WITH latest_price AS (
    SELECT product_id, MAX(change_date) AS latest_date
    FROM Products
    WHERE change_date <= '2019-08-16'
    GROUP BY product_id
)
SELECT p.product_id,
       COALESCE(pr.new_price, 10) AS price
FROM (SELECT DISTINCT product_id FROM Products) p
LEFT JOIN Products pr
  ON p.product_id = pr.product_id
  AND pr.change_date = (SELECT latest_date FROM latest_price WHERE product_id = p.product_id);
```

---

## Interview Tips

- Use CTEs to break complex logic into named steps.
- A CTE improves readability but is not always materialized.
- Recursive CTEs need a base case and a termination path.

## LeetCode Patterns

- Use CTEs to stage rankings, aggregates, or filtered subsets.
- Use multiple CTEs for multi-step transformations.
- Use recursive CTEs for hierarchy problems when supported.
