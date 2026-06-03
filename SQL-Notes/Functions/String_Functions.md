# String Functions

[Back to Master README](../README.md)

## Table of Contents

- [Examples](#examples)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

```sql
LENGTH(str)           -- Length of string
UPPER(str)            -- Uppercase
LOWER(str)            -- Lowercase
TRIM(str)             -- Remove leading/trailing spaces
LTRIM(str)            -- Remove leading spaces
RTRIM(str)            -- Remove trailing spaces
SUBSTRING(str, pos, len)  -- Extract substring
CONCAT(str1, str2)    -- Concatenate strings
REPLACE(str, old, new)    -- Replace substring
LIKE 'pattern%'       -- Pattern matching (% = any, _ = one char)
REGEXP 'pattern'      -- Regular expression matching (MySQL)
```

---

## Examples

```sql
SELECT LOWER(email) AS normalized_email
FROM Users;

SELECT SUBSTRING(name, 1, 1) AS first_initial
FROM Employees;

SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM Employees;
```

## Interview Tips

- Mention that function names vary by dialect, especially CONCAT and substring syntax.
- Normalize case before comparison only when the database collation does not already handle it.
- Avoid wrapping indexed columns in functions in WHERE clauses when performance matters.

## LeetCode Patterns

- Use LOWER or UPPER for case normalization.
- Use SUBSTRING or LEFT to extract prefixes.
- Use CONCAT for required display labels.
