# IN

[Back to Master README](../README.md)

## Table of Contents

- [IN Operator](#in-operator)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## IN Operator

### Definition
`IN` checks if a value matches any value in a given list. It is a cleaner alternative to multiple `OR` conditions on the same column.

### Syntax

```sql
SELECT column_list
FROM table_name
WHERE column_name IN (value1, value2, value3, ...);

-- With NOT
WHERE column_name NOT IN (value1, value2, ...);
```

### Explanation
- The list of values is enclosed in **parentheses** and separated by **commas**.
- `IN` can also accept a subquery as the list (multi-valued subquery).
- `NOT IN` excludes all listed values. Be careful with `NOT IN` and `NULL` - if any value in the list is `NULL`, the result may be empty.

### Example

```sql
-- People named Poe, Hugo, or Dahl
SELECT NAME, BDATE
FROM PERSONS
WHERE NAME IN ('POE', 'HUGO', 'DAHL');

-- Equivalent with OR (less readable)
SELECT NAME, BDATE
FROM PERSONS
WHERE NAME = 'POE'
OR NAME = 'HUGO'
OR NAME = 'DAHL';

-- NOT IN example
SELECT COUNTRY FROM COUNTRIES
WHERE LANGUAGE NOT IN ('English', 'Spanish', 'French');
```

### OR vs IN Comparison

| Feature        | Multiple OR          | IN                    |
|----------------|----------------------|-----------------------|
| Readability    | Verbose              | Clean & concise       |
| Performance    | Same                 | Same                  |
| Subquery       | Not supported        | Supported             |
| NULL handling  | Standard             | Careful with NOT IN   |

### Key Points to Remember

> - `IN` is equivalent to multiple `OR` conditions on the **same column**.
> - `NOT IN` with a subquery that might return `NULL` can return empty results - use `NOT EXISTS` as a safer alternative.
> - `IN` with a subquery is a powerful pattern: `WHERE dept_id IN (SELECT id FROM departments WHERE active = 1)`.

### Common LeetCode Pattern

```sql
-- LC 182: Duplicate Emails
SELECT email
FROM Person
GROUP BY email
HAVING COUNT(*) > 1;

-- Find managers using IN
SELECT name FROM Employee
WHERE id IN (SELECT managerId FROM Employee WHERE managerId IS NOT NULL);
```

---

## Interview Tips

- IN is a compact replacement for repeated OR equality checks.
- Know that NOT IN can behave unexpectedly when the subquery returns NULL.
- Use EXISTS for correlated membership checks when appropriate.

## LeetCode Patterns

- Use IN for small allowed-value lists.
- Prefer NOT EXISTS over NOT IN when nullable values are possible.
- Use IN with subqueries for membership in another table.
