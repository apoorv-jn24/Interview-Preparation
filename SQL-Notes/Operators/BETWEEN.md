# BETWEEN

[Back to Master README](../README.md)

## Table of Contents

- [BETWEEN Operator](#between-operator)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## BETWEEN Operator

### Definition
`BETWEEN` tests whether a value falls within an inclusive range. It is a shorthand for `column >= low AND column <= high`.

### Syntax

```sql
SELECT column_list
FROM table_name
WHERE column_name BETWEEN low_value AND high_value;
```

### Explanation
- The range is **inclusive** - both end points are included.
- The lower value must come first (left of `AND`).
- Works with numbers, dates, and strings.
- Use `NOT BETWEEN` to exclude a range.

### Example

```sql
-- Countries with literacy between 55 and 60 (inclusive)
SELECT COUNTRY, LITERACY
FROM COUNTRIES
WHERE LITERACY BETWEEN 55 AND 60
ORDER BY LITERACY;

-- Equivalent using AND
SELECT COUNTRY, LITERACY
FROM COUNTRIES
WHERE LITERACY >= 55
AND LITERACY <= 60
ORDER BY LITERACY;

-- Date range example
SELECT * FROM orders
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';
```

### Key Points to Remember

> - `BETWEEN` is **inclusive** on both ends.
> - The smaller value must come **first**.
> - Works with numbers, text, and dates.
> - `NOT BETWEEN` excludes a range.

---

## Interview Tips

- BETWEEN is inclusive at both ends.
- Use explicit >= and <= when inclusivity must be obvious.
- For date-time ranges, prefer half-open intervals when timestamps include time.

## LeetCode Patterns

- Use BETWEEN for inclusive numeric ranges.
- Use >= start_date and < next_day for timestamp-safe date filtering.
- Check boundary rows because most wrong answers happen at the edges.
