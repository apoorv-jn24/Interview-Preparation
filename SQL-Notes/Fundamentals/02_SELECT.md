# SELECT

[Back to Master README](../README.md)

## Table of Contents

- [SELECT - Retrieving Data](#select---retrieving-data)
- [Column Aliases - AS](#column-aliases---as)
- [Arithmetic Expressions](#arithmetic-expressions)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## SELECT - Retrieving Data

### Definition
`SELECT` is the fundamental SQL query command used to retrieve data from one or more tables. Every SQL query must have both a `SELECT` clause and a `FROM` clause.

### Syntax

```sql
-- Select all columns
SELECT *
FROM table_name;

-- Select specific columns
SELECT column1, column2, column3
FROM table_name;
```

### Explanation
- `SELECT *` uses an asterisk to retrieve **all** columns from the table.
- Listing column names explicitly controls which columns appear **and in what order**.
- The `FROM` clause always follows the `SELECT` clause.
- Rows are returned in **arbitrary order** unless `ORDER BY` is specified.
- You can repeat a column name in the SELECT list (e.g., `SELECT *, JOB FROM JOBS` will show JOB twice).

### Example

```sql
-- Get all columns from the JOBS table
SELECT *
FROM JOBS;

-- Get only the job title
SELECT TITLE
FROM JOBS;

-- Get title and job code, in that order
SELECT TITLE, JOB
FROM JOBS;
```

**Result of `SELECT TITLE, JOB FROM JOBS`:**

| TITLE       | JOB |
|-------------|-----|
| Scientist   | S   |
| Entertainer | E   |
| Writer      | W   |
| Instructor  | I   |

### Key Points to Remember

> - Every query **must** have both `SELECT` and `FROM`.
> - `SELECT *` is convenient but avoid it in production - it retrieves unnecessary data and breaks if columns are added/removed.
> - Column order in results matches the order you list them in `SELECT`.
> - Rows are returned in **no guaranteed order** without `ORDER BY`.

### Common Interview Questions

1. **What is the difference between `SELECT *` and `SELECT column_name`?**
   `SELECT *` retrieves all columns (slower, less maintainable); listing column names is explicit and safer in production.

2. **Can you select the same column twice?** Yes - `SELECT name, name FROM employees` is valid.

3. **Is SQL case-sensitive for keywords?** No. `select`, `SELECT`, and `Select` are all valid. But string values in `WHERE` clauses **are** case-sensitive in many databases.

### Common LeetCode Pattern

```sql
-- LC 183: Customers Who Never Order
SELECT name AS Customers
FROM Customers
WHERE id NOT IN (SELECT customerId FROM Orders);
```

---

## Column Aliases - AS

### Definition
A column alias assigns a temporary display name to a column or expression in the result set.

### Syntax

```sql
SELECT column_name AS alias_name
FROM table_name;

-- AS keyword is optional in most dialects
SELECT column_name alias_name
FROM table_name;
```

### Explanation
Aliases are defined using the `AS` keyword in the `SELECT` clause. They:
- Rename columns in the output
- Are required when naming computed expressions
- Can be referenced in `ORDER BY` but **not in WHERE** (because WHERE is evaluated before SELECT)

### Example

```sql
SELECT JOB, COUNT(*) AS TOTAL
FROM PERSONS
WHERE GENDER = 'MALE'
GROUP BY JOB
ORDER BY TOTAL DESC;
```

| JOB | TOTAL |
|-----|-------|
| E   | 113   |
| W   | 99    |
| M   | 36    |

### Key Points to Remember

> - Aliases defined in `SELECT` **cannot** be used in `WHERE` (evaluated before SELECT).
> - Aliases **can** be used in `ORDER BY`.
> - Use aliases to make output readable, especially for aggregate columns.
> - In most SQL dialects, the `AS` keyword is optional.

---

## Arithmetic Expressions

### Definition
An arithmetic expression is a formula using numeric values and/or column names with arithmetic operators. SQL evaluates the expression and substitutes the result.

### Operators

| Operator | Meaning    | Example         |
|----------|------------|-----------------|
| `+`      | Add        | `2 + 2`         |
| `-`      | Subtract   | `BDATE - 365`   |
| `*`      | Multiply   | `POP * 1.25`    |
| `/`      | Divide     | `PERCENT / 100` |
| `()`     | Precedence | `2 + (4 / 2)`   |

### Explanation
- Operators follow standard math precedence: `*` and `/` before `+` and `-`.
- Parentheses override precedence.
- Only addition and subtraction are valid with **date** columns.
- Expressions can appear in SELECT, WHERE, ORDER BY, and HAVING clauses.

### Example

```sql
-- Compute population density
SELECT COUNTRY, POP / AREA AS DENSITY
FROM COUNTRIES
ORDER BY DENSITY DESC;

-- Apply a 10% increase to GNP
SELECT COUNTRY, GNP * 1.10 AS PROJECTED_GNP
FROM COUNTRIES;
```

---

## Interview Tips

- Prefer explicit column lists over SELECT * in production and interviews.
- Explain that SELECT controls projection, while FROM controls the row source.
- Mention aliases when output column names must match a requested format.

## LeetCode Patterns

- Use SELECT aliases for required names such as Customers or Department.
- Build the SELECT list after validating joins and filters.
- Use arithmetic expressions for derived metrics such as salary difference or rate.
