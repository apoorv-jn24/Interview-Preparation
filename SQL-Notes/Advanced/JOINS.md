# JOINS

[Back to Master README](../README.md)

## Table of Contents

- [Joins](#joins)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## Joins

### What is a Join?

A join combines columns from two or more tables into a single result table. Tables are joined by matching values in **common columns** (typically primary key and foreign key). The standard join process:
1. Append all rows of the second table to each row of the first (Cartesian product).
2. Filter rows where the common columns match.
3. Eliminate the redundant duplicate column.

### Full Column Names

In multi-table queries, the same column name may exist in multiple tables. Use the format `tablename.columnname` to specify which table's column you mean.

```sql
-- Full column name syntax
PERSONS.COUNTRY
COUNTRIES.COUNTRY
```

---

### INNER JOIN (Equi-Join)

#### Definition
An inner join returns only the rows where the join condition is satisfied in **both** tables. Rows with no match in either table are excluded.

#### Syntax

```sql
-- Explicit JOIN syntax (preferred)
SELECT t1.col, t2.col
FROM table1 AS t1
INNER JOIN table2 AS t2
  ON t1.common_col = t2.common_col;

-- Implicit JOIN syntax (comma-separated FROM clause)
SELECT t1.col, t2.col
FROM table1 AS t1, table2 AS t2
WHERE t1.common_col = t2.common_col;
```

#### Example

```sql
-- Join PERSONS with JOBS to get person name and job title
SELECT PERSON, NAME, PERSONS.JOB, TITLE
FROM PERSONS, JOBS
WHERE PERSONS.JOB = JOBS.JOB;

-- With table aliases (cleaner)
SELECT P.PERSON, P.NAME, P.JOB, J.TITLE
FROM PERSONS AS P
INNER JOIN JOBS AS J
  ON P.JOB = J.JOB;
```

| PERSON | NAME      | JOB | TITLE     |
|--------|-----------|-----|-----------|
| 1      | Einstein  | S   | Scientist |
| 2      | Dickens   | W   | Writer    |
| 3      | Dickinson | W   | Writer    |

```sql
-- Three-table join
SELECT P.NAME, P.COUNTRY, C.GNP, J.TITLE
FROM PERSONS AS P
INNER JOIN COUNTRIES AS C ON P.COUNTRY = C.COUNTRY
INNER JOIN JOBS AS J ON P.JOB = J.JOB
WHERE C.GNP > 1000000;
```

---

### LEFT JOIN (LEFT OUTER JOIN)

#### Definition
Returns all rows from the **left** table, and matching rows from the right table. If no match exists, NULL values fill the right table's columns.

#### Syntax

```sql
SELECT t1.col, t2.col
FROM table1 AS t1
LEFT JOIN table2 AS t2
  ON t1.common_col = t2.common_col;
```

#### Example

```sql
-- All employees, with department name if available
SELECT e.name, d.department_name
FROM employees AS e
LEFT JOIN departments AS d
  ON e.dept_id = d.id;
-- Employees with no department get NULL for department_name
```

---

### RIGHT JOIN (RIGHT OUTER JOIN)

#### Definition
Returns all rows from the **right** table, and matching rows from the left table. Rarely used - a RIGHT JOIN can always be rewritten as a LEFT JOIN by swapping the table order.

---

### FULL OUTER JOIN

#### Definition
Returns all rows from **both** tables. Where no match exists, NULL fills the missing side.

```sql
SELECT t1.col, t2.col
FROM table1 AS t1
FULL OUTER JOIN table2 AS t2
  ON t1.id = t2.id;
```

>  MySQL does not support `FULL OUTER JOIN` natively - simulate it with `UNION` of LEFT JOIN and RIGHT JOIN.

---

### CROSS JOIN

Returns the **Cartesian product** - every row from table1 combined with every row from table2.

```sql
SELECT * FROM table1
CROSS JOIN table2;
-- If table1 has 3 rows and table2 has 4, result has 34 = 12 rows
```

---

### SELF JOIN

Joining a table to **itself** - useful for hierarchical data (employees and managers).

```sql
-- Find employees and their manager's name
SELECT e.name AS employee, m.name AS manager
FROM employees AS e
LEFT JOIN employees AS m
  ON e.manager_id = m.id;
```

---

### Join Types - Visual Summary

```
INNER JOIN         LEFT JOIN          RIGHT JOIN         FULL OUTER JOIN
   A  B             A + (AB)          B + (AB)            A  B
  [A[AB]B]          [A[AB]B]           [A[AB]B]            [A[AB]B]

```

| Join Type       | Returns                                           |
|-----------------|---------------------------------------------------|
| INNER JOIN      | Matching rows in both tables only                 |
| LEFT JOIN       | All rows from left + matching rows from right     |
| RIGHT JOIN      | All rows from right + matching rows from left     |
| FULL OUTER JOIN | All rows from both, NULLs where no match          |
| CROSS JOIN      | Every combination (Cartesian product)             |
| SELF JOIN       | Table joined with itself                          |

---

### Table Aliases

#### Definition
A table alias is a temporary short name for a table, defined in the `FROM` clause using `AS`.

#### Syntax

```sql
FROM table_name AS alias
-- OR (AS is optional in most dialects)
FROM table_name alias
```

#### Explanation
- Aliases shorten full column names: `PERSONS.JOB` becomes `P.JOB`.
- Once an alias is defined, you **must** use the alias (the original name cannot be used in the same query).
- Required for **self joins** (where the same table appears twice).

```sql
-- Without aliases (verbose)
SELECT PERSON, NAME, PERSONS.JOB, TITLE
FROM PERSONS, JOBS
WHERE PERSONS.JOB = JOBS.JOB;

-- With aliases (cleaner)
SELECT PERSON, NAME, P.JOB, TITLE
FROM PERSONS AS P, JOBS AS J
WHERE P.JOB = J.JOB;
```

---

## Interview Tips

- Identify the driving table and relationship before choosing the join type.
- Explain how INNER JOIN, LEFT JOIN, and CROSS JOIN change row counts.
- Always qualify column names when multiple tables share column names.

## LeetCode Patterns

- Use LEFT JOIN plus IS NULL to find missing relationships.
- Use SELF JOIN for comparisons between rows in the same table.
- Use table aliases to keep multi-table queries readable.
