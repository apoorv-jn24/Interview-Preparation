# Subqueries

[Back to Master README](../README.md)

## Table of Contents

- [Subqueries](#subqueries)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## Subqueries

### What is a Subquery?

A subquery (also called a nested query or inner query) is a `SELECT` statement embedded inside another SQL statement. Subqueries are always enclosed in **parentheses**. They are evaluated first, and their result is used by the outer query.

### Single-Valued Subqueries

#### Definition
A single-valued subquery returns **exactly one column and one row**. It can be used anywhere a single value is expected - in a comparison with `=`, `<`, `>`, etc.

#### Syntax

```sql
SELECT column_list
FROM table_name
WHERE column_name OPERATOR (
    SELECT single_column
    FROM table_name
    WHERE condition
);
```

#### Example

```sql
-- Find countries with population above average
SELECT COUNTRY, POP
FROM COUNTRIES
WHERE POP > (SELECT AVG(POP) FROM COUNTRIES);

-- Find the person with the earliest birth date
SELECT NAME, BDATE
FROM PERSONS
WHERE BDATE = (SELECT MIN(BDATE) FROM PERSONS);

-- Find employees earning more than the company average
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

#### Key Points to Remember

> - If the subquery returns more than one row with `=`, SQL throws an error - use `IN` instead.
> - Subqueries are evaluated **once** (unlike correlated subqueries).
> - Can be used in SELECT, FROM, WHERE, and HAVING clauses.

---

### Multi-Valued Subqueries

#### Definition
A multi-valued subquery returns **one column with zero or more rows**. It must be used with the `IN` (or `NOT IN`) operator.

#### Syntax

```sql
SELECT column_list
FROM table_name
WHERE column_name IN (
    SELECT single_column
    FROM table_name
    WHERE condition
);
```

#### Example

```sql
-- Jobs that are held by female persons
SELECT DISTINCT JOB
FROM PERSONS
WHERE GENDER = 'FEMALE';
-- Returns: W, R, E, S, M

-- Find job titles that NO female holds
SELECT JOB, TITLE
FROM JOBS
WHERE JOB NOT IN (
    SELECT DISTINCT JOB
    FROM PERSONS
    WHERE GENDER = 'FEMALE'
);
```

```sql
-- Orders from customers in a specific region
SELECT order_id, total
FROM orders
WHERE customer_id IN (
    SELECT id FROM customers WHERE region = 'North'
);
```

---

### Correlated Subqueries

#### Definition
A correlated subquery references a column from the **outer query**. Unlike regular subqueries, it is executed **once for each row** of the outer query.

#### Syntax

```sql
SELECT column_list
FROM table1 AS t1
WHERE column_name OPERATOR (
    SELECT aggregate(column)
    FROM table1 AS t2
    WHERE t2.grouping_col = t1.grouping_col  -- references outer query!
);
```

#### Explanation
- The subquery refers to a value from the outer query via a **table alias**.
- For each row in the outer query, the subquery runs again with that row's values.
- This makes correlated subqueries potentially slow on large datasets - consider rewriting with a JOIN or window function.

#### Example

```sql
-- Find the oldest person in each job category
SELECT JOB, NAME
FROM PERSONS AS P
WHERE BDATE = (
    SELECT MIN(BDATE)
    FROM PERSONS
    WHERE JOB = P.JOB    -- references outer query's JOB column
);
```

| JOB | NAME     |
|-----|----------|
| E   | Hepburn  |
| W   | Shikabu  |
| M   | Montcalm |
| S   | Magnus   |

```sql
-- Employees earning more than the average for their department
SELECT name, department, salary
FROM employees AS e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department = e.department
);
```

#### Key Points to Remember

> - Correlated subqueries execute **once per row** of the outer query - can be slow.
> - Require a **table alias** on the outer query to reference it inside.
> - Can often be rewritten more efficiently using window functions (`AVG() OVER (PARTITION BY ...)`).

#### Subquery Types Summary

| Type              | Returns         | Used With                | Executes      |
|-------------------|-----------------|--------------------------|---------------|
| Single-valued     | 1 row, 1 col    | `=`, `<`, `>`, etc.      | Once          |
| Multi-valued      | N rows, 1 col   | `IN`, `NOT IN`           | Once          |
| Correlated        | 1 or N rows     | `=`, `IN`, `EXISTS`      | Per outer row |

#### Common LeetCode Pattern

```sql
-- LC 185: Department Top 3 Salaries
SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary
FROM Employee e
JOIN Department d ON e.departmentId = d.id
WHERE (
    SELECT COUNT(DISTINCT salary)
    FROM Employee
    WHERE departmentId = e.departmentId AND salary > e.salary
) < 3;
```

---

## Interview Tips

- Distinguish scalar, multi-row, and correlated subqueries.
- Use EXISTS when testing whether related rows exist.
- Know when a join is clearer or more efficient than a subquery.

## LeetCode Patterns

- Use subqueries for max/min comparisons against an aggregate.
- Use NOT EXISTS for rows absent from another table.
- Use correlated subqueries for row-by-row comparisons.
