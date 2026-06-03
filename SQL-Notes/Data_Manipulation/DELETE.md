# DELETE and Transactions

[Back to Master README](../README.md)

## Table of Contents

- [DELETE](#delete)
- [Transactions](#transactions)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## DELETE

### Definition
Removes one or more rows from a table.

### Syntax

```sql
DELETE FROM table_name
WHERE condition;

-- Delete ALL rows (dangerous!)
DELETE FROM table_name;
```

### WARNING
**Omitting WHERE deletes ALL rows!** Use `TRUNCATE` for clearing all rows (faster), or always verify your WHERE clause.

### Example

```sql
-- Delete a specific country
DELETE FROM COUNTRIES
WHERE COUNTRY = 'Beulah';

-- Delete inactive customers
DELETE FROM customers
WHERE last_order_date < '2020-01-01'
AND status = 'inactive';

-- Delete using a subquery
DELETE FROM orders
WHERE customer_id IN (
    SELECT id FROM customers WHERE status = 'fraud'
);
```

---

## Transactions

### Definition
A transaction is a group of DML operations that are treated as a single unit. Either **all** operations succeed (COMMIT) or **all** are undone (ROLLBACK).

### Syntax

```sql
BEGIN;  -- or START TRANSACTION

INSERT INTO accounts (id, balance) VALUES (1, 1000);
UPDATE accounts SET balance = balance - 500 WHERE id = 1;
UPDATE accounts SET balance = balance + 500 WHERE id = 2;

COMMIT;    -- Make all changes permanent
-- OR
ROLLBACK;  -- Undo all changes since BEGIN
```

### Transaction Properties (ACID)

| Property    | Meaning                                                   |
|-------------|-----------------------------------------------------------|
| Atomicity   | All operations succeed or all are rolled back             |
| Consistency | Database moves from one valid state to another            |
| Isolation   | Concurrent transactions don't interfere with each other   |
| Durability  | Committed changes persist even after system failure       |

### Key Points to Remember

> - `COMMIT` permanently applies all changes since the last commit.
> - `ROLLBACK` undoes all changes since the last commit or BEGIN.
> - DDL statements (CREATE, DROP, ALTER) are **auto-committed** in most databases.
> - Use transactions whenever multiple related DML operations must succeed or fail together.

---

## Interview Tips

- DELETE removes rows; DROP removes objects; TRUNCATE removes all rows quickly.
- Always confirm the WHERE clause before deleting.
- Use transactions when a delete must be reviewed or rolled back.

## LeetCode Patterns

- DELETE is uncommon on LeetCode SQL, but anti-join logic often identifies rows that would be deleted.
- Use transactions in real systems to protect destructive changes.
- Understand cascade behavior when foreign keys are involved.
