# Task 1 — Database Basics

## Objective

Practice fundamental database operations using **MySQL** and **PostgreSQL**.

In this task, I practiced:

* Creating databases
* Creating tables
* Defining columns and data types
* Using primary keys and automatic ID generation
* Inserting employee records
* Altering an existing table
* Deleting records using `WHERE`
* Verifying database and table structures
* Understanding basic syntax differences between MySQL and PostgreSQL

---

# Part 1 — MySQL

## 1. Create Database

Connected to MySQL:

```bash
sudo mysql
```

Created the database:

```sql
CREATE DATABASE test_db;
```

Verified the database:

```sql
SHOW DATABASES;
```

Selected the database:

```sql
USE test_db;
```

Verified the current database:

```sql
SELECT DATABASE();
```

---

## 2. Create Employees Table

Created the `employees` table:

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    position VARCHAR(50),
    salary DECIMAL(10,0)
);
```

Verified the table:

```sql
SHOW TABLES;
```

Checked the table structure:

```sql
DESCRIBE employees;
```

### Table Structure

| Column     | Data Type       | Description                              |
| ---------- | --------------- | ---------------------------------------- |
| `id`       | `INT`           | Primary key with automatic ID generation |
| `name`     | `VARCHAR(100)`  | Employee name                            |
| `position` | `VARCHAR(50)`   | Employee position                        |
| `salary`   | `DECIMAL(10,0)` | Employee salary                          |

---

## 3. Insert Employee Data

Inserted five employees:

```sql
INSERT INTO employees (name, position, salary)
VALUES
('Ali Khan', 'Software Engineer', 75000),
('Sara Ahmed', 'DevOps Engineer', 90000),
('Ahmed Raza', 'Database Administrator', 85000),
('Ayesha Malik', 'Project Manager', 100000),
('Usman Tariq', 'System Adminstrator', 70000);
```

Verified the records:

```sql
SELECT * FROM employees;
```

The `id` values were automatically generated using `AUTO_INCREMENT`.

---

## 4. Alter the Table

Added the `department` column:

```sql
ALTER TABLE employees
ADD COLUMN department VARCHAR(50);
```

Verified the updated structure:

```sql
DESCRIBE employees;
```

The table now contains:

```text
id
name
position
salary
department
```

---

## 5. Delete an Employee

Checked the employee before deleting:

```sql
SELECT * FROM employees
WHERE id = 1;
```

Deleted the employee with `id = 1`:

```sql
DELETE FROM employees
WHERE id = 1;
```

Verified the remaining records:

```sql
SELECT * FROM employees;
```

---

# Part 2 — PostgreSQL

## 1. Connect to PostgreSQL

Connected using the PostgreSQL administrator account:

```bash
sudo -u postgres psql
```

Listed available databases:

```sql
\l
```

---

## 2. Create Database

Created the database:

```sql
CREATE DATABASE test_db;
```

Connected to the database:

```sql
\c test_db
```

Verified that the current connection was using `test_db`.

---

## 3. Create Employees Table

Created the `employees` table:

```sql
CREATE TABLE employees (
    id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(100),
    position VARCHAR(50),
    salary DECIMAL(10,0)
);
```

Verified the table:

```sql
\dt
```

Checked the structure:

```sql
\d employees
```

### Table Structure

| Column     | Data Type       | Description                              |
| ---------- | --------------- | ---------------------------------------- |
| `id`       | `INTEGER`       | Primary key with automatic ID generation |
| `name`     | `VARCHAR(100)`  | Employee name                            |
| `position` | `VARCHAR(50)`   | Employee position                        |
| `salary`   | `NUMERIC(10,0)` | Employee salary                          |

> PostgreSQL displays `DECIMAL` as `NUMERIC`, which is normal because these types are equivalent.

---

## 4. Insert Employee Data

Inserted five employees:

```sql
INSERT INTO employees (name, position, salary)
VALUES
('Ali Khan', 'Software Engineer', 75000),
('Sara Ahmed', 'DevOps Engineer', 90000),
('Ahmed Raza', 'Database Administrator', 85000),
('Ayesha Malik', 'Project Manager', 100000),
('Usman Tariq', 'System Administrator', 70000);
```

Verified the records:

```sql
SELECT * FROM employees;
```

The IDs were automatically generated using:

```sql
GENERATED ALWAYS AS IDENTITY
```

---

## 5. Alter the Table

Added the `department` column:

```sql
ALTER TABLE employees
ADD COLUMN department VARCHAR(50);
```

Verified the structure:

```sql
\d employees
```

The table now contains:

```text
id
name
position
salary
department
```

---

## 6. Delete an Employee

Deleted the employee with `id = 5`:

```sql
DELETE FROM employees
WHERE id = 5;
```

Verified the remaining records:

```sql
SELECT * FROM employees;
```

---

# MySQL vs PostgreSQL

During this task, I practiced the differences between MySQL and PostgreSQL commands.

| Operation       | MySQL                 | PostgreSQL                     |
| --------------- | --------------------- | ------------------------------ |
| Connect         | `mysql`               | `psql`                         |
| List databases  | `SHOW DATABASES;`     | `\l`                           |
| Select database | `USE test_db;`        | `\c test_db`                   |
| List tables     | `SHOW TABLES;`        | `\dt`                          |
| Describe table  | `DESCRIBE employees;` | `\d employees`                 |
| Automatic ID    | `AUTO_INCREMENT`      | `GENERATED ALWAYS AS IDENTITY` |
| Exit            | `exit;`               | `\q`                           |

---

# Key Concepts Learned

### Database

A database is an organized collection of data that can be stored, managed, and queried.

### Table

A table stores related information using rows and columns.

### Primary Key

A primary key uniquely identifies each record in a table.

In this task, `id` was used as the primary key.

### `AUTO_INCREMENT`

MySQL uses `AUTO_INCREMENT` to automatically generate ID values.

### `GENERATED ALWAYS AS IDENTITY`

PostgreSQL uses identity columns to automatically generate ID values.

### `INSERT`

Used to add records:

```sql
INSERT INTO employees (...);
```

### `SELECT`

Used to retrieve records:

```sql
SELECT * FROM employees;
```

### `ALTER TABLE`

Used to modify the structure of an existing table:

```sql
ALTER TABLE employees
ADD COLUMN department VARCHAR(50);
```

### `DELETE`

Used to remove records:

```sql
DELETE FROM employees
WHERE id = 5;
```

### `WHERE`

Used to specify which records should be affected by a query.

For example:

```sql
DELETE FROM employees
WHERE id = 5;
```

deletes only the employee with ID `5`.

---

# Task Completion

* [x] Created `test_db` in MySQL
* [x] Created `test_db` in PostgreSQL
* [x] Created `employees` table in MySQL
* [x] Created `employees` table in PostgreSQL
* [x] Defined primary keys
* [x] Used automatic ID generation
* [x] Inserted five employee records in MySQL
* [x] Inserted five employee records in PostgreSQL
* [x] Added `department` column in MySQL
* [x] Added `department` column in PostgreSQL
* [x] Deleted an employee record by ID in MySQL
* [x] Deleted an employee record by ID in PostgreSQL
* [x] Verified database and table structures
* [x] Verified inserted and deleted records
* [x] Practiced MySQL and PostgreSQL CLI commands

## Conclusion

This task provided practical experience with the fundamentals of relational databases using **MySQL and PostgreSQL**. I practiced creating databases and tables, defining data types and primary keys, inserting and deleting records, modifying table structures, and verifying database operations.

I also learned the key syntax differences between MySQL and PostgreSQL while performing the same database operations in both database systems.
