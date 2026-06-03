# WHERE

[Back to Master README](../README.md)

## Table of Contents

- [WHERE Clause](#where-clause)
- [NULL Handling Cheat Sheet](#null-handling-cheat-sheet)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## WHERE Clause

### Definition
The `WHERE` clause filters rows based on a condition. Only rows where the condition evaluates to `TRUE` are included in the result.

### Syntax

```sql
SELECT column_list
FROM table_name
WHERE condition;
```

### Explanation
A condition (comparison) consists of:
- A **column name** on the left
- A **comparison operator** in the middle
- A **value** on the right

The WHERE clause must follow the FROM clause.

### Comparison Operators

| Operator | Meaning                  | Example                      |
|----------|--------------------------|------------------------------|
| `=`      | Equal to                 | `NAME = 'EINSTEIN'`          |
| `<>`     | Not equal to             | `BDATE <> '1944-05-02'`      |
| `<`      | Less than                | `POP < 100000`               |
| `<=`     | Less than or equal to    | `NAME <= 'O''Grady'`         |
| `>`      | Greater than             | `AREA > 999`                 |
| `>=`     | Greater than or equal to | `BDATE >= '1962-06-19'`      |

### Example

```sql
-- Rows where area is greater than 3 million sq km
SELECT COUNTRY, AREA
FROM COUNTRIES
WHERE AREA > 3000000;

-- Rows for a specific country
SELECT NAME, COUNTRY
FROM PERSONS
WHERE COUNTRY = 'RUSSIA';

-- Rows for very old historical persons
SELECT NAME, BDATE
FROM PERSONS
WHERE BDATE < '1300-01-01';
```

### Key Points to Remember

> - The `WHERE` clause always **follows** the `FROM` clause.
> - String comparisons are in **single quotes**: `WHERE NAME = 'Smith'`.
> - Date comparisons also use single quotes: `WHERE bdate > '2000-01-01'`.
> - `<>` means "not equal to" - some databases also support `!=`.
> - Column names are **never** in quotes.

### Common Interview Questions

1. **Can you use aggregate functions in WHERE?** No - use `HAVING` instead (evaluated after grouping).
2. **What is the difference between WHERE and HAVING?** WHERE filters individual rows before grouping; HAVING filters groups after aggregation.

### Common LeetCode Pattern

```sql
-- LC 196: Delete Duplicate Emails
DELETE FROM Person
WHERE id NOT IN (
    SELECT MIN(id) FROM Person GROUP BY email
);
```

---

## NULL Handling Cheat Sheet

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

## Interview Tips

- Explain that WHERE filters rows before grouping.
- Call out that comparisons with NULL require IS NULL or IS NOT NULL.
- Use parentheses when combining predicates to make intent unambiguous.

## LeetCode Patterns

- Filter early for status, date, amount, or id conditions.
- Use NOT EXISTS or LEFT JOIN for anti-join problems when NULL can appear.
- Test boundary conditions for equal values and missing values.
