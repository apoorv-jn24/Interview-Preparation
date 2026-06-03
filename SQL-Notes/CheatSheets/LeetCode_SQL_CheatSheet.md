# LeetCode SQL Cheat Sheet

[Back to Master README](../README.md)

## Table of Contents

- [SELECT](#select)
- [WHERE](#where)
- [GROUP BY](#group-by)
- [HAVING](#having)
- [JOINS](#joins)
- [Subqueries](#subqueries)
- [Window Functions](#window-functions)
- [CASE WHEN](#case-when)
- [Common Patterns](#common-patterns)

## SELECT

```sql
SELECT column1, column2
FROM table_name;

SELECT column_name AS output_name
FROM table_name;
```

- Select only required columns.
- Use aliases to match expected LeetCode output names.
- Use expressions for derived values.

```sql
SELECT    [DISTINCT] column_list            -- 5th: what to show
FROM      table_name                        -- 1st: where data lives
[JOIN     other_table ON condition]         -- 1st: bring in related data
WHERE     row_filter_condition              -- 2nd: filter individual rows
GROUP BY  grouping_columns                  -- 3rd: aggregate into groups
HAVING    group_filter_condition            -- 4th: filter groups
ORDER BY  sort_columns [ASC|DESC]           -- 6th: sort the output
[LIMIT    n OFFSET m];                      -- 7th: paginate results
```

## WHERE

```sql
SELECT name
FROM Customers
WHERE referee_id <> 2 OR referee_id IS NULL;
```

- WHERE filters rows before grouping.
- Use IS NULL for missing values.
- Use parentheses when combining AND and OR.

## GROUP BY

```sql
SELECT customer_id, COUNT(*) AS order_count
FROM Orders
GROUP BY customer_id;
```

- Group by the entity requested in the result.
- Every non-aggregated SELECT column should appear in GROUP BY.
- Use COUNT(*), COUNT(column), SUM, AVG, MIN, and MAX.

## HAVING

```sql
SELECT email
FROM Person
GROUP BY email
HAVING COUNT(*) > 1;
```

- HAVING filters aggregate groups.
- Use WHERE before GROUP BY for row filters.
- Use HAVING for duplicate and threshold problems.

## JOINS

```sql
SELECT c.name AS Customers
FROM Customers c
LEFT JOIN Orders o
  ON c.id = o.customerId
WHERE o.id IS NULL;
```

- INNER JOIN keeps matching rows only.
- LEFT JOIN keeps all rows from the left table.
- LEFT JOIN plus IS NULL is the classic missing-record pattern.

## Subqueries

```sql
SELECT name
FROM Employee
WHERE salary > (
  SELECT AVG(salary)
  FROM Employee
);
```

- Use scalar subqueries for single comparison values.
- Use IN for multi-row membership.
- Prefer NOT EXISTS over NOT IN when NULL may appear.

## Window Functions

```sql
SELECT employee_id, department_id, salary
FROM (
  SELECT employee_id,
         department_id,
         salary,
         DENSE_RANK() OVER (
           PARTITION BY department_id
           ORDER BY salary DESC
         ) AS salary_rank
  FROM Employee
) ranked
WHERE salary_rank = 1;
```

- Use ROW_NUMBER for one row per group.
- Use RANK or DENSE_RANK when ties matter.
- Use LAG and LEAD for previous/next row comparisons.

## CASE WHEN

```sql
SELECT user_id,
       SUM(CASE WHEN action = 'confirmed' THEN 1 ELSE 0 END) AS confirmed_count,
       SUM(CASE WHEN action = 'timeout' THEN 1 ELSE 0 END) AS timeout_count
FROM Confirmations
GROUP BY user_id;
```

- CASE WHEN creates conditional values inside SELECT, SUM, AVG, and ORDER BY.
- Use ELSE 0 for conditional counts.
- Use ELSE NULL when AVG should ignore non-matching rows.

```sql
SELECT
SUM(CASE WHEN status = 'shipped'   THEN 1 ELSE 0 END) AS shipped,
SUM(CASE WHEN status = 'pending'   THEN 1 ELSE 0 END) AS pending,
SUM(CASE WHEN status = 'cancelled' THEN 1 ELSE 0 END) AS cancelled
FROM orders;
```

## Common Patterns

### Finding Duplicates

```sql
-- Find emails that appear more than once
SELECT email
FROM Person
GROUP BY email
HAVING COUNT(*) > 1;
```

### Nth Highest Value

```sql
-- N-th highest salary (N=2  second highest)
SELECT DISTINCT salary
FROM Employee
ORDER BY salary DESC
LIMIT 1 OFFSET N-1;

-- Using DENSE_RANK
SELECT salary FROM (
SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
FROM Employee
) t WHERE rnk = N;
```

### Rows Not Present in Another Table

```sql
-- Customers who never placed an order
SELECT name FROM Customers
WHERE id NOT IN (SELECT customerId FROM Orders);

-- Using LEFT JOIN (handles NULLs safely)
SELECT c.name FROM Customers c
LEFT JOIN Orders o ON c.id = o.customerId
WHERE o.id IS NULL;
```

### Running Totals

```sql
SELECT id, amount,
SUM(amount) OVER (ORDER BY id) AS running_total
FROM transactions;
```

### Ranking Within Groups

```sql
SELECT department, name, salary,
RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank
FROM employees;
```

### Most Recent Record Per Group

```sql
-- Most recent order per customer
SELECT customer_id, order_id, order_date
FROM (
SELECT customer_id, order_id, order_date,
ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) AS rn
FROM orders
) t
WHERE rn = 1;
```

### Consecutive Records

```sql
-- Find IDs with 3 consecutive same values
SELECT DISTINCT num AS ConsecutiveNums
FROM (
SELECT num,
LEAD(num, 1) OVER (ORDER BY id) AS n1,
LEAD(num, 2) OVER (ORDER BY id) AS n2
FROM Logs
) t
WHERE num = n1 AND num = n2;
```

### Self Join for Comparisons

```sql
-- Find employees earning more than their manager
SELECT e.name AS employee
FROM employees e
JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

### Percentage of Total

```sql
SELECT department,
COUNT(*) AS headcount,
ROUND(100.0 * COUNT(*) / SUM(COUNT(*)) OVER (), 2) AS pct_of_total
FROM employees
GROUP BY department;
```

### Anti-Join Variant

```sql
SELECT c.name
FROM Customers c
LEFT JOIN Orders o
  ON c.id = o.customerId
WHERE o.customerId IS NULL;
```
