# GROUP BY and HAVING

[Back to Master README](../README.md)

## Table of Contents

- [GROUP BY & HAVING](#group-by-having)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## GROUP BY & HAVING

### GROUP BY

#### Definition
`GROUP BY` divides the result set into groups based on one or more columns, then applies aggregate functions to each group separately. Without `GROUP BY`, aggregate functions operate on the entire table.

#### Syntax

```sql
SELECT column1, column2, AGGREGATE_FUNCTION(column3)
FROM table_name
WHERE condition
GROUP BY column1, column2
ORDER BY column1;
```

#### Explanation
- Every **non-aggregated** column in the SELECT clause must appear in the `GROUP BY` clause.
- SQL divides the table into groups, calculates the aggregate for each group, and returns one row per group.
- `GROUP BY` comes **after** the `WHERE` clause and **before** `ORDER BY`.
- You can group by multiple columns to create sub-groups.

#### Example

```sql
-- Count persons by job type (single grouping column)
SELECT JOB, COUNT(*) AS TOTAL
FROM PERSONS
WHERE GENDER = 'MALE'
GROUP BY JOB
ORDER BY JOB;
```

| JOB | TOTAL |
|-----|-------|
| B   | 28    |
| E   | 113   |
| M   | 36    |
| S   | 28    |
| W   | 99    |

```sql
-- Count persons by job AND country (multi-column grouping)
SELECT JOB, COUNTRY, COUNT(*) AS TOTAL
FROM PERSONS
WHERE GENDER = 'MALE'
GROUP BY JOB, COUNTRY
ORDER BY JOB, COUNTRY;
```

#### Key Points to Remember

> - Non-aggregate columns in SELECT **must** be in GROUP BY.
> - `WHERE` filters rows **before** grouping; `HAVING` filters **after** grouping.
> - `GROUP BY` produces one output row per unique combination of grouping columns.
> - `NULL` values are grouped together in most SQL databases.

#### Common Interview Questions

1. **What is the difference between WHERE and HAVING?**
   `WHERE` filters rows before aggregation; `HAVING` filters groups after aggregation.
2. **Can you GROUP BY a column not in SELECT?** Yes - though it's rarely useful.
3. **What happens if you SELECT a non-grouped, non-aggregated column?** It produces an error in standard SQL (MySQL sometimes allows it with non-deterministic results).

---

### HAVING

#### Definition
`HAVING` filters **groups** after `GROUP BY` has been applied. It is the aggregate equivalent of `WHERE`.

#### Syntax

```sql
SELECT column1, AGGREGATE_FUNCTION(column2) AS alias
FROM table_name
WHERE row_condition
GROUP BY column1
HAVING group_condition
ORDER BY alias;
```

#### Explanation
- `HAVING` is placed **after** `GROUP BY` and **before** `ORDER BY`.
- It can reference aggregate functions directly (unlike `WHERE`).
- It can also reference column aliases defined in SELECT (in most modern databases).
- `WHERE` and `HAVING` can coexist: WHERE filters rows first, then GROUP BY groups them, then HAVING filters groups.

#### Example

```sql
-- Show only job types with more than 30 male persons
SELECT JOB, COUNT(*) AS TOTAL
FROM PERSONS
WHERE GENDER = 'MALE'
GROUP BY JOB
HAVING TOTAL > 30
ORDER BY TOTAL DESC;
```

| JOB | TOTAL |
|-----|-------|
| E   | 113   |
| W   | 99    |
| M   | 36    |

```sql
-- Departments with average salary over 60000
SELECT department, AVG(salary) AS avg_sal
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000;

-- Countries with more than 5 cities
SELECT country, COUNT(*) AS city_count
FROM cities
GROUP BY country
HAVING COUNT(*) > 5
ORDER BY city_count DESC;
```

#### WHERE vs HAVING Comparison

| Feature            | WHERE                        | HAVING                        |
|--------------------|------------------------------|-------------------------------|
| Filters            | Individual rows              | Groups (after aggregation)    |
| When applied       | Before GROUP BY              | After GROUP BY                |
| Can use aggregates |  No                         |  Yes                         |
| Can use columns    |  Yes                        |  Yes (grouped columns)       |
| Position in query  | After FROM                   | After GROUP BY                |

#### Key Points to Remember

> - `HAVING` filters **groups**, not rows.
> - Use `WHERE` to filter rows before grouping; use `HAVING` to filter after grouping.
> - `HAVING` can reference aggregate functions directly.
> - A query can have both `WHERE` and `HAVING`.

#### Common LeetCode Pattern

```sql
-- LC 596: Classes with >= 5 Students
SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5;

-- LC 586: Customer with the Most Orders
SELECT customer_number
FROM Orders
GROUP BY customer_number
ORDER BY COUNT(*) DESC
LIMIT 1;
```

---

## Interview Tips

- WHERE filters rows before grouping; HAVING filters groups after aggregation.
- Every non-aggregated SELECT column should appear in GROUP BY.
- Explain aggregate edge cases, especially COUNT(column) ignoring NULL.

## LeetCode Patterns

- Use GROUP BY to calculate per-customer, per-product, or per-day metrics.
- Use HAVING for duplicate detection and threshold filters.
- Use COUNT(*) > 1 to find repeated values.
