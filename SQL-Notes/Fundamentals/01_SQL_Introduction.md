# SQL Introduction

[Back to Master README](../README.md)

## Table of Contents

- [What is SQL?](#what-is-sql)
- [SQL Statement Categories](#sql-statement-categories)
- [What is a Relational Database?](#what-is-a-relational-database)
- [SQL Data Types & Value Syntax](#sql-data-types-value-syntax)
- [Interview Tips](#interview-tips)
- [LeetCode Patterns](#leetcode-patterns)

## What is SQL?

SQL (Structured Query Language) is the standard language used to interact with relational databases. Originally developed by IBM in 1974 and standardized by ANSI/ISO in 1986-1987, SQL is now supported by over 100 commercial database products including MySQL, PostgreSQL, SQL Server, Oracle, and SQLite.

A SQL statement is a collection of **clauses**, **keywords**, and **parameters** that perform a specific function. Keywords are written in uppercase by convention; parameters (table names, column names) are in lowercase or mixed case.

## SQL Statement Categories

| Category    | Statement | Purpose                         |
|-------------|-----------|----------------------------------|
| Query       | SELECT    | Retrieve rows from one or more tables |
| Maintenance | INSERT    | Add new rows to a table          |
| Maintenance | UPDATE    | Modify existing rows             |
| Maintenance | DELETE    | Remove rows                      |
| Definition  | CREATE    | Add tables, indexes, views       |
| Definition  | DROP      | Remove tables, indexes, views    |

## What is a Relational Database?

A relational database is a collection of data organized into **tables**. Each table has:
- **Columns** (vertical) - identified by name, unique within a table
- **Rows** (horizontal) - identified by their data values, not position

**Example - PERSONS table:**

| PERSON | NAME      | BDATE      | GENDER | COUNTRY | JOB |
|--------|-----------|------------|--------|---------|-----|
| 1      | Einstein  | 1879-03-20 | Male   | Germany | S   |
| 2      | Dickens   | 1812-07-22 | Male   | England | W   |
| 3      | Dickinson | 1830-07-15 | Female | USA     | W   |

>  **Key Point:** Table names and column names are case-insensitive in most SQL dialects, but by convention they are written in uppercase in this guide.

---

## SQL Data Types & Value Syntax

When writing SQL, values must be entered correctly based on their type:

| Category    | Description           | Examples                           |
|-------------|-----------------------|------------------------------------|
| NUMERIC     | Integers and decimals | `3`, `+12`, `-7`, `3.14`, `-0.96`  |
| NON-NUMERIC | Text strings          | `'Chamberlin'`, `'We love SQL'`    |
| DATE        | Date values           | `'1996-01-01'`, `'2024-12-31'`     |

**Rules:**
- **Numbers** - entered as-is. No commas, dollar signs, or percent signs allowed.
- **Strings** - always enclosed in **single quotes** `'like this'`. Use two consecutive single quotes to represent one quote inside a string: `'I don''t'`.
- **Dates** - use `'yyyy-mm-dd'` format, enclosed in single quotes.

---

## Interview Tips

- Define SQL as the language for querying and changing relational data.
- Separate query, DML, DDL, and constraint concepts before discussing syntax.
- Use table, row, column, primary key, and foreign key precisely.

## LeetCode Patterns

- Start every problem by identifying the source table and required output columns.
- Use aliases to match exact expected column names.
- Remember that LeetCode usually validates both data and output column labels.
