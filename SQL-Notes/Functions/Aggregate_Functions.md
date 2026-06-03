# Aggregate Functions

[Back to Master README](../README.md)

## Table of Contents

- [Aggregate Functions](#aggregate-functions)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## Aggregate Functions

### What Are Aggregate Functions?

Aggregate functions (also called statistical functions) take an entire column (or expression) as input and return a **single summary value**. They operate on groups of rows - either the whole table or groups defined by `GROUP BY`.

### The Five Standard Aggregate Functions

| Function    | Meaning                       | Works On        | Ignores NULLs? |
|-------------|-------------------------------|-----------------|----------------|
| `COUNT(*)`  | Count of all rows             | Any             | No             |
| `COUNT(col)`| Count of non-null rows        | Any             | Yes            |
| `COUNT(DISTINCT col)` | Count of unique non-null values | Any | Yes       |
| `SUM(col)`  | Total of all values           | Numeric only    | Yes            |
| `MIN(col)`  | Smallest value                | Any             | Yes            |
| `MAX(col)`  | Largest value                 | Any             | Yes            |
| `AVG(col)`  | Average of all values         | Numeric only    | Yes            |

#### Syntax

```sql
SELECT AGGREGATE_FUNCTION(column_or_expression)
FROM table_name
WHERE condition;
```

#### Explanation
- Aggregate functions **cannot** be used in the `WHERE` clause (use `HAVING` instead).
- `SUM` and `AVG` work only on **numeric columns**.
- `COUNT`, `MIN`, and `MAX` work on **any type** - numbers, strings, dates.
- All aggregate functions (except `COUNT(*)`) **ignore NULL values**.
- Can use arithmetic inside: `AVG(POP / AREA)`.

#### Example

```sql
-- Count all rows in the table
SELECT COUNT(*) AS total_persons
FROM PERSONS;

-- Count only rows with a job code
SELECT COUNT(JOB) AS persons_with_job
FROM PERSONS;

-- Count distinct countries
SELECT COUNT(DISTINCT COUNTRY) AS unique_countries
FROM PERSONS;

-- Total population of all countries
SELECT SUM(POP) AS world_population
FROM COUNTRIES;

-- Smallest and largest area
SELECT MIN(AREA) AS smallest, MAX(AREA) AS largest
FROM COUNTRIES;

-- Average GNP
SELECT AVG(GNP) AS avg_gnp
FROM COUNTRIES;

-- Average population density
SELECT AVG(POP / AREA) AS avg_density
FROM COUNTRIES;
```

#### Key Points to Remember

> - `COUNT(*)`  `COUNT(column)` - `COUNT(*)` includes rows with nulls; `COUNT(col)` does not.
> - All aggregates except `COUNT(*)` **skip NULLs** automatically.
> - Aggregate functions return **one row** unless combined with `GROUP BY`.
> - You **cannot** mix a column name with an aggregate in SELECT without `GROUP BY`.
> - `SUM` and `AVG` only work on **numeric** types.

#### Common Interview Questions

1. **What does `COUNT(*)` return vs `COUNT(column)`?** `COUNT(*)` counts all rows; `COUNT(col)` counts rows where col is not null.
2. **What happens when you `AVG` a column with NULLs?** NULLs are ignored - AVG is computed over non-null values only.
3. **Can you use aggregate functions in WHERE?** No - use `HAVING` for filtering on aggregate results.

#### Common LeetCode Pattern

```sql
-- LC 620: Not Boring Movies
SELECT * FROM cinema
WHERE id % 2 = 1 AND description <> 'boring'
ORDER BY rating DESC;

-- LC 1251: Average Selling Price
SELECT p.product_id,
       ROUND(SUM(p.price * u.units) / SUM(u.units), 2) AS average_price
FROM Prices p
JOIN UnitsSold u
  ON p.product_id = u.product_id
  AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY p.product_id;
```

---

## Interview Tips

- COUNT(*) counts rows, while COUNT(column) ignores NULL values.
- Aggregates return one row per group when GROUP BY is present.
- Use COUNT(DISTINCT column) for unique counts.

## LeetCode Patterns

- Use COUNT, SUM, AVG, MIN, and MAX for reporting questions.
- Combine aggregates with GROUP BY for per-user or per-department metrics.
- Use COALESCE when aggregate output should display zero instead of NULL.
