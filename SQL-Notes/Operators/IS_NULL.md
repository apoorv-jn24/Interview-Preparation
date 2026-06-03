# IS NULL

[Back to Master README](../README.md)

## Table of Contents

- [IS NULL Operator](#is-null-operator)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## IS NULL Operator

### Definition
`IS NULL` tests whether a column value is null (missing/unknown). You cannot use `= NULL` to check for nulls.

### Syntax

```sql
SELECT column_list
FROM table_name
WHERE column_name IS NULL;

-- Opposite
WHERE column_name IS NOT NULL;
```

### Explanation
A **null value** means the data is missing, unknown, or does not apply. Null is:
- **NOT** the same as zero (numeric)
- **NOT** the same as an empty string (`''`)
- Not equal to another null - two nulls are not necessarily equal
- Incompatible with arithmetic operations (any arithmetic with null returns null)
- Incompatible with `=` comparison - `WHERE col = NULL` always returns nothing

### Example

```sql
-- Countries with no population recorded
SELECT COUNTRY, POP
FROM COUNTRIES
WHERE POP IS NULL;

-- Persons from Iran with unknown birth dates
SELECT NAME, BDATE
FROM PERSONS
WHERE COUNTRY = 'IRAN'
AND BDATE IS NULL;

-- Filter out nulls
SELECT * FROM employees
WHERE manager_id IS NOT NULL;
```

### NULL in Logic (Three-Valued Logic)

| Expression       | Result  |
|------------------|---------|
| NULL = NULL      | NULL (not TRUE!) |
| NULL <> NULL     | NULL    |
| NULL = 0         | NULL    |
| NULL IS NULL     | TRUE   |
| NULL IS NOT NULL | FALSE   |

### Key Points to Remember

> - Always use `IS NULL` / `IS NOT NULL`, never `= NULL`.
> - `NULL` propagates - any operation with `NULL` returns `NULL`.
> - `COUNT(*)` counts all rows including nulls; `COUNT(column)` skips nulls.
> - In `NOT IN` subqueries, a single `NULL` in the list causes the entire `NOT IN` to return nothing.

### Common Interview Questions

1. **What is the difference between `NULL`, `0`, and empty string?** `NULL` = unknown/missing; `0` = a valid numeric zero; `''` = a valid empty string.
2. **What does `COUNT(*)` vs `COUNT(column)` return for null values?** `COUNT(*)` counts all rows; `COUNT(column)` counts only non-null values.

### Common LeetCode Pattern

```sql
-- LC 584: Find Customer Referee
SELECT name FROM Customer
WHERE referee_id <> 2 OR referee_id IS NULL;
-- Can't use just <> 2 because NULL <> 2 evaluates to NULL, not TRUE
```

---

## Interview Tips

- NULL means unknown or missing, not zero or empty string.
- Use IS NULL and IS NOT NULL instead of = NULL.
- Explain three-valued logic: TRUE, FALSE, and UNKNOWN.

## LeetCode Patterns

- Use IS NULL after LEFT JOIN to find missing matches.
- Use COALESCE for default values in reporting output.
- Avoid NOT IN if the subquery may return NULL.
