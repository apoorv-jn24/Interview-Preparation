# UNION

[Back to Master README](../README.md)

## Table of Contents

- [UNION](#union)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## UNION

### Definition
`UNION` combines the results of two or more `SELECT` statements into a single result set, removing duplicates by default.

### Syntax

```sql
SELECT column_list FROM table1 WHERE condition1
UNION
SELECT column_list FROM table2 WHERE condition2
ORDER BY 1;
```

### Explanation
- The number and **data types** of columns must match across all SELECT statements.
- `UNION` removes duplicate rows automatically.
- `UNION ALL` keeps all rows including duplicates (faster).
- `ORDER BY` must appear only in the **last** SELECT, and must use **column positions**, not names.

### Example

```sql
-- Countries with population > 200M OR countries where Atheism > 30%
SELECT COUNTRY FROM COUNTRIES WHERE POP > 200000000
UNION
SELECT COUNTRY FROM RELIGIONS WHERE RELIGION = 'ATHEISM' AND PERCENT > 30
ORDER BY 1;
```

| UNION vs UNION ALL |
|--------------------|
| `UNION` - removes duplicates (slower) |
| `UNION ALL` - keeps duplicates (faster) |

### Key Points to Remember

> - Column count and types must **match** in all SELECT statements.
> - `UNION` deduplicates; `UNION ALL` does not.
> - `ORDER BY` goes only in the **last** query, using column positions.

### Common Interview Questions

1. **What is the difference between JOIN and UNION?** JOIN combines columns (horizontal); UNION combines rows (vertical).
2. **When would you use UNION ALL over UNION?** When duplicates are acceptable and performance matters (UNION ALL is faster as it skips dedup).

### Common LeetCode Pattern

```sql
-- LC 1965: Employees With Missing Information
SELECT employee_id FROM Employees WHERE employee_id NOT IN (SELECT employee_id FROM Salaries)
UNION
SELECT employee_id FROM Salaries WHERE employee_id NOT IN (SELECT employee_id FROM Employees)
ORDER BY employee_id;
```

---

## Interview Tips

- UNION removes duplicates; UNION ALL preserves duplicates.
- Both queries must return the same number of compatible columns.
- Use UNION ALL unless duplicate removal is required.

## LeetCode Patterns

- Use UNION to combine symmetric result sets.
- Use UNION ALL for event streams or logs where duplicates matter.
- Apply ORDER BY only once at the end of the combined query.
