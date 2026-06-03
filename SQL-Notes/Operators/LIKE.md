# LIKE

[Back to Master README](../README.md)

## Table of Contents

- [LIKE Operator - Pattern Matching](#like-operator---pattern-matching)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## LIKE Operator - Pattern Matching

### Definition
`LIKE` is used in a `WHERE` clause to search for a specified pattern in a column. It uses two wildcard characters.

### Syntax

```sql
SELECT column_list
FROM table_name
WHERE column_name LIKE 'pattern';
```

### Wildcard Characters

| Wildcard | Meaning                          | Example Pattern | Matches           |
|----------|----------------------------------|-----------------|-------------------|
| `%`      | Zero or more unknown characters  | `'Z%'`          | Zola, Zimbalist, Zwingli |
| `_`      | Exactly one unknown character    | `'EINST__N'`    | Einstein          |

### Explanation
- Patterns must always be **enclosed in single quotes**.
- `%` matches any sequence of characters, including none.
- `_` matches exactly one character.
- Wildcards can appear anywhere in the pattern: beginning, middle, or end.
- Most databases support `ILIKE` for case-insensitive pattern matching.

### Example

```sql
-- Names starting with 'Z'
SELECT NAME, COUNTRY
FROM PERSONS
WHERE NAME LIKE 'Z%';
-- Returns: Zola, Zimbalist, Zwingli

-- Names matching 'Einstein' with wildcards
SELECT NAME
FROM PERSONS
WHERE NAME LIKE 'EINST_ _N';
-- Returns: Einstein

-- Names containing 'ST' followed by two characters and 'N'
SELECT NAME, COUNTRY
FROM PERSONS
WHERE NAME LIKE '%ST_ _N%';
-- Returns: Einstein, Springsteen, Steinbeck, Silverstein

-- Names ending in 'son'
SELECT NAME FROM EMPLOYEES WHERE NAME LIKE '%son';
```

### Key Points to Remember

> - `%` = **any number** of characters (including zero).
> - `_` = **exactly one** character.
> - `LIKE` is case-sensitive in most databases (use `ILIKE` in PostgreSQL for case-insensitive).
> - Use `NOT LIKE` to exclude patterns.
> - Escape a literal `%` or `_` with an escape character: `LIKE '100\%' ESCAPE '\'`.

### Common Interview Questions

1. **What is the difference between `%` and `_` in LIKE?** `%` matches zero or more characters; `_` matches exactly one.
2. **How do you search for a literal `%`?** Use an escape clause: `WHERE col LIKE '50\%' ESCAPE '\'`.

### Common LeetCode Pattern

```sql
-- Find patients with conditions starting with 'DIAB1'
SELECT patient_id, patient_name, conditions
FROM Patients
WHERE conditions LIKE 'DIAB1%' OR conditions LIKE '% DIAB1%';
```

---

## Interview Tips

- Explain percent (%) and underscore (_) wildcards clearly.
- Mention that leading wildcards often prevent normal index usage.
- Clarify database-specific case sensitivity when matching strings.

## LeetCode Patterns

- Use LIKE for prefix, suffix, and contains filters.
- Use NOT LIKE for exclusion rules.
- Combine LIKE with LOWER or UPPER only when case-insensitive matching is needed.
