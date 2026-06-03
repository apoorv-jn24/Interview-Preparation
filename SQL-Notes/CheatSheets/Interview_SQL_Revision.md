# Interview SQL Revision

[Back to Master README](../README.md)

## Table of Contents

- [Revision Map](#revision-map)
- [Query Clause Order](#query-clause-order)
- [High-Frequency Interview Questions](#high-frequency-interview-questions)
- [NULL Handling](#null-handling)
- [Performance Notes](#performance-notes)
- [Operator Reference](#operator-reference)
- [Practice Problems](#practice-problems)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## Revision Map

- Fundamentals: SELECT, WHERE, DISTINCT, ORDER BY.
- Operators: LIKE, BETWEEN, IN, IS NULL, AND/OR/NOT.
- Aggregation: COUNT, SUM, AVG, MIN, MAX with GROUP BY and HAVING.
- Relationships: INNER JOIN, LEFT JOIN, SELF JOIN, UNION.
- Advanced solving: subqueries, CTEs, and window functions.
- Schema topics: tables, constraints, indexes, and views.
- DML safety: INSERT, UPDATE, DELETE, and transactions.

## Query Clause Order

```sql
SELECT column_list
FROM table_name
JOIN other_table ON join_condition
WHERE row_filter
GROUP BY group_columns
HAVING group_filter
ORDER BY sort_columns;
```

## High-Frequency Interview Questions

- Difference between WHERE and HAVING.
- Difference between INNER JOIN and LEFT JOIN.
- Difference between RANK and DENSE_RANK.
- Difference between DELETE, TRUNCATE, and DROP.
- Why COUNT(*) and COUNT(column) can return different values.
- How indexes improve reads and slow writes.
- Why NOT IN can fail when NULL appears.

## NULL Handling

```sql
-- Replace NULL with a default value
COALESCE(column, 'default_value')
IFNULL(column, 0)          -- MySQL
ISNULL(column, 0)          -- SQL Server
NVL(column, 0)             -- Oracle

-- Conditional NULLification
NULLIF(col1, col2)         -- Returns NULL if col1 = col2

-- NULL-safe filtering
WHERE column IS NULL
WHERE column IS NOT NULL

-- Avoid NOT IN with NULLs
-- Dangerous:
WHERE id NOT IN (SELECT manager_id FROM employees)
-- manager_id may contain NULL  returns nothing

-- Safe alternative:
WHERE id NOT IN (SELECT manager_id FROM employees WHERE manager_id IS NOT NULL)
-- OR use NOT EXISTS:
WHERE NOT EXISTS (SELECT 1 FROM employees WHERE manager_id = e.id)
```

---

## Performance Notes

1. **Use indexes** - join on indexed columns when possible.
2. **Filter early** - apply `WHERE` conditions before `JOIN` when possible.
3. **Prefer `EXISTS` over `IN`** for large subqueries - `EXISTS` short-circuits.
4. **Avoid `SELECT *`** in production - specify columns.
5. **Use `UNION ALL` instead of `UNION`** when duplicates are acceptable - avoids deduplication overhead.
6. **Window functions > correlated subqueries** - correlated subqueries run once per row; window functions are set-based and much faster.
7. **`NOT EXISTS` over `NOT IN`** when subquery may return NULLs.
8. **Limit result sets** - use `LIMIT` during development.
9. **Use `EXPLAIN`** to inspect query execution plans.

---

## Operator Reference

| Category       | Operator       | Example                             |
|----------------|----------------|-------------------------------------|
| Comparison     | `=`            | `WHERE salary = 50000`              |
| Comparison     | `<>` / `!=`    | `WHERE status <> 'active'`          |
| Comparison     | `< > <= >=`    | `WHERE age >= 18`                   |
| Range          | `BETWEEN`      | `WHERE price BETWEEN 10 AND 100`    |
| List           | `IN`           | `WHERE dept IN ('HR', 'IT')`        |
| Pattern        | `LIKE`         | `WHERE name LIKE 'A%'`              |
| NULL check     | `IS NULL`      | `WHERE phone IS NULL`               |
| Logical        | `AND OR NOT`   | `WHERE a=1 AND b=2`                 |
| Negation       | `NOT IN`       | `WHERE id NOT IN (1, 2, 3)`         |
| Existence      | `EXISTS`       | `WHERE EXISTS (SELECT 1 ...)`       |

---

## Practice Problems

| # | Problem | Key Concept |
|---|---------|-------------|
| 175 | Combine Two Tables | LEFT JOIN |
| 176 | Second Highest Salary | Subquery / LIMIT OFFSET |
| 177 | Nth Highest Salary | DENSE_RANK / LIMIT |
| 178 | Rank Scores | DENSE_RANK() |
| 180 | Consecutive Numbers | LAG/LEAD or Self-Join |
| 181 | Employees Earning More Than Manager | Self Join |
| 182 | Duplicate Emails | GROUP BY + HAVING |
| 183 | Customers Who Never Order | NOT IN / LEFT JOIN |
| 184 | Department Highest Salary | Window Function |
| 185 | Department Top 3 Salaries | DENSE_RANK() |
| 196 | Delete Duplicate Emails | DELETE + Subquery |
| 197 | Rising Temperature | LAG() or Self Join |
| 262 | Trips and Users | Conditional aggregation |
| 550 | Game Play Analysis IV | Window Function |
| 585 | Investments in 2016 | Correlated Subquery |

---

*Notes compiled from the SQL Manual (Relational Systems Corporation, v1.2) and enriched with modern SQL patterns for technical interviews and competitive programming.*

* Repository: `SQL Notes for Interviews & LeetCode`*

## Interview Tips

- Answer concept questions with a definition, a use case, and one edge case.
- When optimizing, discuss indexes, row counts, selectivity, and avoiding unnecessary columns.
- For query-writing interviews, narrate the data grain at each step.

## LeetCode Patterns

- Practice anti-joins, duplicate detection, ranking, running totals, and latest-row-per-group.
- Use small CTEs to debug complex answers.
- Validate tie behavior and NULL behavior with sample rows.
