# AND, OR, and NOT

[Back to Master README](../README.md)

## Table of Contents

- [AND Operator](#and-operator)
- [OR Operator](#or-operator)
- [Operator Precedence & NOT](#operator-precedence-not)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## AND Operator

### Definition
`AND` combines two or more conditions. **All** conditions must be true for a row to be included.

### Syntax

```sql
SELECT column_list
FROM table_name
WHERE condition1 AND condition2;
```

### Example

```sql
-- People whose names start with 'A' AND were born after 1900
SELECT NAME, BDATE
FROM PERSONS
WHERE NAME LIKE 'A%'
AND BDATE >= '1900-01-01';

-- Countries with GNP between 1000 and 2000 (using AND)
SELECT COUNTRY, GNP
FROM COUNTRIES
WHERE GNP >= 1000
AND GNP <= 2000;
```

### Key Points to Remember

> - Both conditions must be `TRUE` - if either is `FALSE` or `NULL`, the row is excluded.
> - Chain multiple `AND`s: `WHERE a=1 AND b=2 AND c=3`.
> - `AND` has higher precedence than `OR`. Use parentheses to control evaluation.

---

## OR Operator

### Definition
`OR` combines two conditions. **At least one** condition must be true for a row to be included.

### Syntax

```sql
SELECT column_list
FROM table_name
WHERE condition1 OR condition2;
```

### Example

```sql
-- People named Luther OR Calvin
SELECT NAME, BDATE
FROM PERSONS
WHERE NAME = 'LUTHER'
OR NAME = 'CALVIN';

-- Countries in Ghana, USA, or Fiji
SELECT COUNTRY, LANGUAGE
FROM COUNTRIES
WHERE COUNTRY = 'GHANA'
OR COUNTRY = 'USA'
OR COUNTRY = 'FIJI';
```

### Key Points to Remember

> - `AND` has **higher precedence** than `OR`. `a OR b AND c` is evaluated as `a OR (b AND c)`.
> - Always use parentheses when mixing `AND` and `OR` to avoid bugs.
> - `IN` is a cleaner alternative to multiple `OR` conditions on the same column.

### AND vs OR Comparison

| Operator | Both TRUE | One TRUE | Both FALSE |
|----------|-----------|----------|------------|
| AND      |  TRUE   |  FALSE |  FALSE   |
| OR       |  TRUE   |  TRUE  |  FALSE   |

---

## Operator Precedence & NOT

When multiple operators appear in a single `WHERE` clause, SQL evaluates them in this order:

| Priority | Operator              |
|----------|-----------------------|
| 1 (High) | Arithmetic: `* /`     |
| 2        | Arithmetic: `+ -`     |
| 3        | Comparisons: `= <> < > <= >=` |
| 4        | `NOT`                 |
| 5        | `AND`                 |
| 6 (Low)  | `OR`                  |

**`NOT` negates a condition:**

```sql
-- Rows where country is NOT the USA
WHERE NOT COUNTRY = 'USA'
-- Same as:
WHERE COUNTRY <> 'USA'

-- More useful with IN/LIKE/BETWEEN
WHERE NOT NAME LIKE 'A%'
WHERE NOT LITERACY BETWEEN 80 AND 90
WHERE NOT COUNTRY IN ('USA', 'England', 'Germany')
```

>  **Always use parentheses** when mixing `AND` and `OR` to make intent explicit:
> ```sql
> -- Ambiguous (AND binds tighter):
> WHERE a = 1 OR b = 2 AND c = 3
> -- Clear with parentheses:
> WHERE a = 1 OR (b = 2 AND c = 3)
> ```

---

## Interview Tips

- AND is evaluated before OR unless parentheses override precedence.
- Use parentheses to communicate intent even when precedence is known.
- Apply De Morgan's laws when simplifying NOT conditions.

## LeetCode Patterns

- Group conditions by business rule before writing SQL.
- Use NOT with EXISTS for anti-join questions.
- Be careful with NULL because NOT UNKNOWN is still UNKNOWN.
