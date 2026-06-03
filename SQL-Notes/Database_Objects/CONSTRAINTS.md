# Constraints

[Back to Master README](../README.md)

## Table of Contents

- [Constraints](#constraints)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## Constraints

### What are Constraints?

Constraints are rules enforced on columns to maintain **data integrity**. They are specified in the `CREATE TABLE` statement and enforced automatically by the database during INSERT, UPDATE, and DELETE operations.

### NOT NULL

Prevents null values in a column.

```sql
CREATE TABLE employees (
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

### DEFAULT

Provides an automatic value when no value is specified on INSERT.

```sql
CREATE TABLE orders (
    status      VARCHAR(20) DEFAULT 'pending',
    created_at  TIMESTAMP   DEFAULT CURRENT_TIMESTAMP
);
```

### UNIQUE

Prevents duplicate values in a column (or combination of columns).

```sql
CREATE TABLE users (
    email VARCHAR(100) UNIQUE,
    username VARCHAR(50) UNIQUE
);

-- Multi-column UNIQUE
UNIQUE (first_name, last_name)
```

### CHECK

Validates that column values satisfy a specified condition.

```sql
CREATE TABLE WRITERS (
    GENDER CHAR(6) DEFAULT 'Male',
    SALES  NUMERIC(9, 2) DEFAULT 0,
    CHECK (GENDER IN ('Male', 'Female')),
    CHECK (SALES >= 0)
);

-- Modern examples
CREATE TABLE products (
    price    DECIMAL(10, 2) CHECK (price >= 0),
    quantity INT            CHECK (quantity >= 0),
    rating   NUMERIC(3,1)   CHECK (rating BETWEEN 0 AND 10)
);
```

### PRIMARY KEY

Uniquely identifies each row. Combines `NOT NULL` + `UNIQUE`. A table can have only **one** primary key (but it can span multiple columns).

```sql
-- Single-column primary key
CREATE TABLE countries (
    country_code CHAR(3) PRIMARY KEY,
    country_name VARCHAR(100) NOT NULL
);

-- Composite primary key
CREATE TABLE order_items (
    order_id   INT,
    product_id INT,
    quantity   INT,
    PRIMARY KEY (order_id, product_id)
);
```

### FOREIGN KEY

Ensures referential integrity - values in this column must match a primary key in another (referenced) table.

```sql
CREATE TABLE WRITERS (
    NAME    CHAR(20),
    COUNTRY CHAR(20),
    PRIMARY KEY (NAME),
    FOREIGN KEY (COUNTRY) REFERENCES COUNTRIES (COUNTRY)
);

-- Modern syntax with cascade options
CREATE TABLE orders (
    id          INT PRIMARY KEY,
    customer_id INT NOT NULL,
    FOREIGN KEY (customer_id)
        REFERENCES customers(id)
        ON DELETE CASCADE      -- delete orders if customer deleted
        ON UPDATE CASCADE      -- update FK if customer PK changes
);
```

#### Constraint Summary

| Constraint    | Purpose                                         | NULL Allowed |
|---------------|-------------------------------------------------|--------------|
| `NOT NULL`    | Column must have a value                        |  No         |
| `DEFAULT`     | Provides a fallback value                       | Depends      |
| `UNIQUE`      | No duplicate values (NULLs are usually allowed) |  Usually    |
| `CHECK`       | Value must satisfy a condition                  |  Yes        |
| `PRIMARY KEY` | Uniquely identifies each row (NOT NULL + UNIQUE)|  No         |
| `FOREIGN KEY` | Values must exist in another table              |  Yes        |

#### Key Points to Remember

> - `PRIMARY KEY` = `NOT NULL` + `UNIQUE`. Only **one** per table.
> - `FOREIGN KEY` enforces referential integrity between tables.
> - `CHECK` constraints validate data at the database level (not just application level).
> - `DEFAULT` runs only when the column is omitted from an `INSERT`.
> - `UNIQUE` allows one `NULL` in most databases; `PRIMARY KEY` does not allow `NULL`.

---

## Interview Tips

- Explain constraints as database-enforced rules.
- Differentiate PRIMARY KEY, UNIQUE, FOREIGN KEY, CHECK, DEFAULT, and NOT NULL.
- Mention how constraints protect data integrity beyond application code.

## LeetCode Patterns

- Use primary keys to identify unique rows.
- Use foreign keys to infer join paths.
- Use nullability to decide between INNER JOIN and LEFT JOIN.
