# Window Functions

[Back to Master README](../README.md)

## Table of Contents

- [Window Functions](#window-functions)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## Window Functions

### What are Window Functions?

Window functions perform calculations across a **set of rows related to the current row** - called a "window" - without collapsing those rows into a single output row (unlike aggregate functions with `GROUP BY`). They are among the most powerful tools in modern SQL.

### Syntax

```sql
function_name(expression)
OVER (
    [PARTITION BY column_list]
    [ORDER BY column_list]
    [ROWS/RANGE BETWEEN frame_start AND frame_end]
)
```

| Clause         | Purpose                                                        |
|----------------|----------------------------------------------------------------|
| `PARTITION BY` | Divides rows into groups (like GROUP BY, but keeps all rows)  |
| `ORDER BY`     | Defines the order within each partition                        |
| `ROWS BETWEEN` | Defines the sliding window frame                              |

---

### ROW_NUMBER, RANK, DENSE_RANK

| Function       | Ties handled         | Gaps in sequence? |
|----------------|----------------------|-------------------|
| `ROW_NUMBER()` | Each row gets unique | N/A               |
| `RANK()`       | Same rank for ties   | Yes (1,1,3)       |
| `DENSE_RANK()` | Same rank for ties   | No (1,1,2)        |

```sql
-- Rank employees by salary within each department
SELECT
    name,
    department,
    salary,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_num,
    RANK()       OVER (PARTITION BY department ORDER BY salary DESC) AS rnk,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rnk
FROM employees;
```

**Example output:**

| name  | dept | salary | row_num | rnk | dense_rnk |
|-------|------|--------|---------|-----|-----------|
| Alice | IT   | 90000  | 1       | 1   | 1         |
| Bob   | IT   | 80000  | 2       | 2   | 2         |
| Carol | IT   | 80000  | 3       | 2   | 2         |
| Dave  | IT   | 70000  | 4       | 4   | 3         |

---

### LAG and LEAD

`LAG` accesses a value from a **previous** row. `LEAD` accesses a value from a **following** row.

```sql
-- Compare each month's sales to the previous month
SELECT
    month,
    sales,
    LAG(sales, 1, 0)  OVER (ORDER BY month) AS prev_month_sales,
    LEAD(sales, 1, 0) OVER (ORDER BY month) AS next_month_sales,
    sales - LAG(sales) OVER (ORDER BY month) AS month_over_month_change
FROM monthly_sales;
```

**Syntax:** `LAG(column, offset, default_value)`

---

### Running Totals and Moving Averages

```sql
-- Running total of sales
SELECT
    order_date,
    amount,
    SUM(amount) OVER (ORDER BY order_date) AS running_total,
    AVG(amount) OVER (ORDER BY order_date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS rolling_7day_avg
FROM sales;
```

---

### NTILE

Divides rows into a specified number of roughly equal buckets.

```sql
SELECT name, salary,
       NTILE(4) OVER (ORDER BY salary DESC) AS quartile
FROM employees;
```

---

### FIRST_VALUE and LAST_VALUE

```sql
SELECT name, salary,
       FIRST_VALUE(name) OVER (PARTITION BY dept ORDER BY salary DESC) AS top_earner,
       LAST_VALUE(name)  OVER (PARTITION BY dept ORDER BY salary DESC
                               ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS lowest_earner
FROM employees;
```

---

### Window Function Quick Reference

| Function                | Use Case                                  |
|-------------------------|-------------------------------------------|
| `ROW_NUMBER()`          | Unique sequential number per row          |
| `RANK()`                | Rank with gaps on ties                    |
| `DENSE_RANK()`          | Rank without gaps on ties                 |
| `NTILE(n)`              | Divide into n buckets                     |
| `LAG(col, n)`           | Access value from n rows back             |
| `LEAD(col, n)`          | Access value from n rows ahead            |
| `SUM() OVER()`          | Running total                             |
| `AVG() OVER()`          | Moving average                            |
| `FIRST_VALUE()`         | First value in window                     |
| `LAST_VALUE()`          | Last value in window                      |
| `PERCENT_RANK()`        | Relative rank as 0-1 percentage           |
| `CUME_DIST()`           | Cumulative distribution                   |

#### Key Points to Remember

> - Window functions do **not** reduce the number of rows (unlike GROUP BY + aggregate).
> - `PARTITION BY` is optional - if omitted, the window is the entire result set.
> - `ORDER BY` inside `OVER()` is independent of the query's `ORDER BY`.
> - `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` is the default frame for running totals.
> - Cannot use window functions in `WHERE` or `HAVING` - wrap in a subquery or CTE.

#### Common Interview Questions

1. **What is the difference between ROW_NUMBER, RANK, and DENSE_RANK?** With ties: ROW_NUMBER gives unique numbers; RANK skips numbers after ties; DENSE_RANK does not skip.
2. **Can window functions be used in WHERE?** No - use a subquery or CTE to filter on window function results.
3. **What is the difference between aggregate functions and window functions?** Aggregates collapse rows to one; window functions keep all rows and add a calculated column.

#### Common LeetCode Pattern

```sql
-- LC 185: Department Top 3 Salaries (window function approach)
SELECT Department, Employee, Salary
FROM (
    SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary,
           DENSE_RANK() OVER (PARTITION BY d.id ORDER BY e.salary DESC) AS rnk
    FROM Employee e JOIN Department d ON e.departmentId = d.id
) ranked
WHERE rnk <= 3;

-- LC 180: Consecutive Numbers
SELECT DISTINCT num AS ConsecutiveNums
FROM (
    SELECT num,
           LEAD(num, 1) OVER (ORDER BY id) AS next1,
           LEAD(num, 2) OVER (ORDER BY id) AS next2
    FROM Logs
) t
WHERE num = next1 AND num = next2;
```

---

## Interview Tips

- Window functions calculate across related rows without collapsing the result set.
- PARTITION BY defines groups; ORDER BY defines sequence within each partition.
- Know the difference between ROW_NUMBER, RANK, and DENSE_RANK.

## LeetCode Patterns

- Use ROW_NUMBER for latest row per group.
- Use DENSE_RANK for nth highest salary with ties.
- Use LAG and LEAD for consecutive records and trend comparisons.
