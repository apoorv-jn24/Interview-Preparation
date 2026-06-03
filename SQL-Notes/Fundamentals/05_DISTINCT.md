# DISTINCT

[Back to Master README](../README.md)

## Table of Contents

- [DISTINCT - Removing Duplicate Rows](#distinct---removing-duplicate-rows)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## DISTINCT - Removing Duplicate Rows

### Definition
`DISTINCT` eliminates duplicate rows from query results. It is placed immediately after `SELECT`.

### Syntax

```sql
SELECT DISTINCT column_name
FROM table_name;
```

### Explanation
Duplicate rows can appear when selecting a subset of columns (e.g., selecting only `RELIGION` from a table with many countries). `DISTINCT` ensures each unique combination of selected column values appears only once.

- `DISTINCT` applies to the **entire row** of selected columns, not just one column.
- It may only be entered **once** per query, immediately after `SELECT`.
- It works across multiple columns: `SELECT DISTINCT col1, col2` returns unique combinations of both.

### Example

```sql
-- Without DISTINCT - returns duplicates
SELECT RELIGION
FROM RELIGIONS
WHERE PERCENT = 100
ORDER BY RELIGION;
-- Result: Catholic, Catholic, Catholic, Catholic, Eastern Orthodox, Lutheran, Muslim, Muslim, Muslim, Sunni Muslim

-- With DISTINCT - clean result
SELECT DISTINCT RELIGION
FROM RELIGIONS
WHERE PERCENT = 100
ORDER BY RELIGION;
-- Result: Catholic, Eastern Orthodox, Lutheran, Muslim, Sunni Muslim
```

### Key Points to Remember

> - `DISTINCT` is written **right after** `SELECT`: `SELECT DISTINCT`.
> - It operates on the **combination** of all selected columns.
> - Can only appear **once** per SELECT clause.
> - Useful for finding unique values: countries, departments, categories.

### Common Interview Questions

1. **What is the difference between `DISTINCT` and `GROUP BY`?**
   Both remove duplicates. `DISTINCT` is simpler for just returning unique rows. `GROUP BY` is used when you need aggregate calculations alongside unique values.

2. **Can you use `DISTINCT` with aggregate functions?** Yes: `COUNT(DISTINCT column)` counts unique non-null values.

### Common LeetCode Pattern

```sql
-- Find all unique email domains, or count unique customers
SELECT COUNT(DISTINCT customer_id) AS unique_customers
FROM orders;
```

---

## Interview Tips

- DISTINCT applies to the whole selected row, not only the first column.
- Use COUNT(DISTINCT column) when asked for unique counts.
- Prefer GROUP BY when aggregate calculations are required.

## LeetCode Patterns

- Use DISTINCT to remove duplicate ids, emails, countries, or departments.
- Use COUNT(DISTINCT user_id) for active user and retention questions.
- Check whether duplicate removal hides a join mistake before submitting.
