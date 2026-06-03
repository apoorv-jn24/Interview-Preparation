# CREATE TABLE and DDL Commands

[Back to Master README](../README.md)

## Table of Contents

- [DDL Commands](#ddl-commands)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## DDL Commands

DDL (Data Definition Language) commands define and manage the structure of database objects. DDL statements are **auto-committed** - they take effect immediately and cannot be rolled back in most databases.

### CREATE TABLE

#### Definition
Creates a new table in the database with specified columns, data types, and optional constraints.

#### Syntax

```sql
CREATE TABLE table_name (
    column1 datatype [size] [constraints],
    column2 datatype [size] [constraints],
    ...
    [table_constraints]
);
```

#### Common Data Types

| Data Type          | Description                         | Example              |
|--------------------|-------------------------------------|----------------------|
| `CHAR(n)`          | Fixed-length string of n characters | `CHAR(20)`           |
| `VARCHAR(n)`       | Variable-length string, max n chars | `VARCHAR(255)`       |
| `INT` / `INTEGER`  | Whole number                        | `INT`                |
| `NUMERIC(p, s)`    | Decimal number, p digits, s decimal | `NUMERIC(9, 2)`      |
| `DECIMAL(p, s)`    | Same as NUMERIC                     | `DECIMAL(10, 2)`     |
| `DATE`             | Date value                          | `DATE`               |
| `TIMESTAMP`        | Date and time                       | `TIMESTAMP`          |
| `BOOLEAN`          | True/false                          | `BOOLEAN`            |
| `TEXT`             | Unlimited-length string             | `TEXT`               |

#### Example

```sql
-- Create a WRITERS table
CREATE TABLE WRITERS (
    NAME    CHAR(20),
    BDATE   DATE,
    GENDER  CHAR(6),
    COUNTRY CHAR(20),
    SALES   NUMERIC(9, 2)
);

-- Modern equivalent with more appropriate types
CREATE TABLE employees (
    id          INT PRIMARY KEY AUTO_INCREMENT,
    first_name  VARCHAR(50) NOT NULL,
    last_name   VARCHAR(50) NOT NULL,
    email       VARCHAR(100) UNIQUE NOT NULL,
    hire_date   DATE NOT NULL,
    salary      DECIMAL(10, 2) DEFAULT 0.00,
    dept_id     INT
);
```

#### Key Points to Remember

> - Table names must start with a letter and can contain letters, digits, underscores.
> - Column definitions are comma-separated inside parentheses.
> - Data types vary slightly between databases (e.g., `AUTO_INCREMENT` in MySQL vs `SERIAL` in PostgreSQL).
> - `CREATE TABLE IF NOT EXISTS` avoids an error if the table already exists.

---

### DROP TABLE

#### Definition
Permanently removes a table and all its data from the database.

#### Syntax

```sql
DROP TABLE table_name;

-- Safe version (no error if table doesn't exist)
DROP TABLE IF EXISTS table_name;
```

>  **Irreversible!** Once dropped, all data is permanently lost. Always verify before executing.

---

### ALTER TABLE

Modifies the structure of an existing table.

```sql
-- Add a new column
ALTER TABLE employees ADD COLUMN phone VARCHAR(20);

-- Drop a column
ALTER TABLE employees DROP COLUMN phone;

-- Modify a column's data type
ALTER TABLE employees MODIFY COLUMN salary DECIMAL(12, 2);

-- Rename a column (MySQL 8+)
ALTER TABLE employees RENAME COLUMN old_name TO new_name;

-- Add a constraint
ALTER TABLE employees ADD CONSTRAINT fk_dept
    FOREIGN KEY (dept_id) REFERENCES departments(id);
```

---

### TRUNCATE TABLE

Removes **all rows** from a table but keeps the table structure intact. Faster than `DELETE` for clearing all rows.

```sql
TRUNCATE TABLE table_name;
```

| Command         | Removes Structure | Removes Data | Can Rollback | Speed    |
|-----------------|-------------------|--------------|--------------|----------|
| `DROP TABLE`    |  Yes             |  Yes        |  No (DDL)  | Fast     |
| `TRUNCATE`      |  No              |  Yes        |  Usually   | Fast     |
| `DELETE`        |  No              |  Yes        |  Yes (DML) | Slower   |

---

## Interview Tips

- Separate DDL commands from DML commands.
- Mention that DDL changes schema and may auto-commit depending on the database.
- Choose data types that match domain rules, not only current sample data.

## LeetCode Patterns

- LeetCode rarely asks DDL directly, but schemas define join keys and nullability.
- Read column types before using string, date, or numeric functions.
- Use schema constraints to infer expected one-to-one or one-to-many relationships.
