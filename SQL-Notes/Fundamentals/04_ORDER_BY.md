# ORDER BY

[Back to Master README](../README.md)

## Table of Contents

- [Sorting](#sorting)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## Sorting

### ORDER BY Clause

#### Definition
`ORDER BY` sorts the rows of a result table. It is always the **last** clause in a query.

#### Syntax

```sql
SELECT column_list
FROM table_name
WHERE condition
ORDER BY column_name [ASC | DESC];

-- Sort by multiple columns
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC];

-- Sort by column position (position in SELECT list)
ORDER BY 2 DESC;
```

#### Explanation
- `ASC` = ascending order (A-Z, 0-9, oldest to newest) - this is the **default**.
- `DESC` = descending order (Z-A, 9-0, newest to oldest).
- You can sort by **column name** or **column position** in the SELECT list.
- You can sort by **multiple columns** - the first column is the primary sort; subsequent columns sort within ties.
- `NULL` values are typically sorted last in ascending order (varies by database).
- You can reference a **column alias** in `ORDER BY`.

#### Example

```sql
-- Sort countries by area (smallest first - default ascending)
SELECT COUNTRY, AREA
FROM COUNTRIES
ORDER BY AREA;

-- Sort by area descending (largest first)
SELECT COUNTRY, AREA
FROM COUNTRIES
ORDER BY AREA DESC;

-- Sort by column position (AREA is 2nd in SELECT)
SELECT COUNTRY, AREA
FROM COUNTRIES
ORDER BY 2 DESC;

-- Multi-column sort: sort by last name, then first name
SELECT last_name, first_name, department
FROM employees
ORDER BY last_name ASC, first_name ASC;

-- Sort using alias
SELECT JOB, COUNT(*) AS TOTAL
FROM PERSONS
GROUP BY JOB
ORDER BY TOTAL DESC;
```

#### Key Points to Remember

> - `ORDER BY` must be the **last clause** in any query.
> - Default order is **ascending** (`ASC`) - no need to specify it explicitly.
> - Sorting by column position (`ORDER BY 2`) is convenient but fragile - if columns change, the sort breaks.
> - Multi-column sort: primary sort first, tie-breaking columns after.
> - `ORDER BY` happens **after** `WHERE`, `GROUP BY`, and `HAVING`.

#### Common Interview Questions

1. **What is the default sort order?** Ascending (ASC).
2. **Can you sort by a column not in the SELECT clause?** Yes, in most databases.
3. **Where does ORDER BY sit in the query execution order?** It's the very last step, after all filtering, grouping, and having clauses.

#### Query Clause Order (Syntax vs. Execution)

| Written Order | Clause       | Execution Order |
|---------------|--------------|-----------------|
| 1             | SELECT       | 5               |
| 2             | FROM         | 1               |
| 3             | WHERE        | 2               |
| 4             | GROUP BY     | 3               |
| 5             | HAVING       | 4               |
| 6             | ORDER BY     | 6               |

#### Common LeetCode Pattern

```sql
-- LC 176: Second Highest Salary
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);

-- Using ORDER BY with LIMIT
SELECT salary AS SecondHighestSalary
FROM Employee
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

---

## Interview Tips

- Say that SQL result order is not guaranteed unless ORDER BY is present.
- Mention tie-breakers when ranking, pagination, or top-N output must be deterministic.
- Know how NULL sorting differs by database engine.

## LeetCode Patterns

- Use ORDER BY with LIMIT/OFFSET or window functions for top-N questions.
- Add secondary sort columns when rows can tie.
- Do not add ORDER BY unless the problem requires ordered output.
