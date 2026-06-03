# Date Functions

[Back to Master README](../README.md)

## Table of Contents

- [Examples](#examples)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

```sql
-- Current date/time
CURRENT_DATE             -- '2024-06-03'
CURRENT_TIMESTAMP        -- '2024-06-03 14:30:00'
NOW()                    -- MySQL: same as CURRENT_TIMESTAMP

-- Extract parts
YEAR(date)               -- MySQL
EXTRACT(YEAR FROM date)  -- Standard SQL
MONTH(date)
DAY(date)

-- Date arithmetic
DATE_ADD(date, INTERVAL 7 DAY)    -- MySQL
date + INTERVAL '7 days'          -- PostgreSQL
DATEADD(day, 7, date)             -- SQL Server

-- Date difference
DATEDIFF(end_date, start_date)    -- MySQL (returns days)
AGE(end_date, start_date)         -- PostgreSQL

-- Format
DATE_FORMAT(date, '%Y-%m-%d')     -- MySQL
TO_CHAR(date, 'YYYY-MM-DD')       -- PostgreSQL/Oracle
```

---

## Examples

```sql
SELECT order_id
FROM Orders
WHERE order_date >= '2024-01-01'
  AND order_date < '2024-02-01';

SELECT user_id, DATEDIFF(MAX(activity_date), MIN(activity_date)) AS active_span_days
FROM Activity
GROUP BY user_id;
```

## Interview Tips

- Use date functions carefully because syntax differs across MySQL, PostgreSQL, SQL Server, and Oracle.
- Prefer range predicates over formatting a date in WHERE.
- For timestamp columns, half-open intervals avoid missing rows late in the day.

## LeetCode Patterns

- Use DATEDIFF for consecutive-day and retention problems.
- Use date ranges for monthly or yearly activity filters.
- Use grouping by date parts for daily, monthly, or yearly summaries.
