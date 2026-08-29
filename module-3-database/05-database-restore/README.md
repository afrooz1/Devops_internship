# Task 5 — Database Restore

## Objective

Practice restoring databases from previously created database dump files in both MySQL and PostgreSQL.

In this task, I practiced:

* Restoring a MySQL database from a `.sql` dump file
* Restoring a PostgreSQL database from a `.sql` dump file
* Restoring into a separate database for testing
* Verifying restored tables
* Verifying restored data using `SELECT`
* Understanding the relationship between backup and restore

---


# Part 1 — MySQL Database Restore

## Step 1 — Create Restore Database

I created a separate database named `test_db_restore` so that the original `test_db` database would not be modified.

```bash
sudo mysql
```

Inside MySQL:

```sql
CREATE DATABASE test_db_restore;
```

I verified the database using:

```sql
SHOW DATABASES;
```

---

## Step 2 — Restore MySQL Dump

The MySQL dump created in the previous task was:

```text
test_db_dump.sql
```

I restored it into the new database using:

```bash
sudo mysql test_db_restore < ~/test_db_dump.sql
```

### Command Explanation

```text
sudo mysql test_db_restore < ~/test_db_dump.sql
       │          │                  │
       │          │                  └── Dump file
       │          └── Destination database
       └── MySQL client
```

The `<` operator passes the contents of the SQL dump file to MySQL.

---

## Step 3 — Verify MySQL Restore

I connected to MySQL:

```bash
sudo mysql
```

Then selected the restored database:

```sql
USE test_db_restore;
```

I checked the available tables:

```sql
SHOW TABLES;
```

Output:

```text
+---------------------------+
| Tables_in_test_db_restore |
+---------------------------+
| employees                 |
+---------------------------+
```

This confirmed that the `employees` table was restored successfully.

---

## Step 4 — Verify MySQL Data

I ran:

```sql
SELECT * FROM employees;
```

The restored data was:

```text
+----+--------------+------------------------+--------+------------+
| id | name         | position               | salary | department |
+----+--------------+------------------------+--------+------------+
|  2 | Sara Ahmed   | DevOps Engineer        |  90000 | DevOps     |
|  3 | Ahmed Raza   | Database Administrator |  85000 | Database   |
|  4 | Ayesha Malik | Project Manager        | 100000 | Management |
|  5 | Usman Tariq  | System Adminstrator    |  70000 | IT         |
+----+--------------+------------------------+--------+------------+
```

The data matched the contents of the MySQL dump created during the previous task.

---

# Part 2 — PostgreSQL Database Restore

## Step 1 — Create Restore Database

I opened PostgreSQL using:

```bash
sudo -u postgres psql
```

Then created a separate restore database:

```sql
CREATE DATABASE test_db_restore;
```

This database is separate from the original PostgreSQL `test_db`.

---

## Step 2 — Restore PostgreSQL Dump

The PostgreSQL dump created in the previous task was:

```text
postgres_test_db_dump.sql
```

I restored it using:

```bash
sudo -u postgres psql test_db_restore < ~/postgres_test_db_dump.sql
```

The command restored the database objects and data from the dump file into `test_db_restore`.

---

## Step 3 — Connect to Restored Database

I connected to the restore database:

```bash
sudo -u postgres psql
```

Initially, I was connected to the `postgres` database.

I then connected to the restored database using:

```sql
\c test_db_restore
```

The prompt changed to:

```text
test_db_restore=#
```

This confirmed that I was working inside the restored database.

---

## Step 4 — Verify PostgreSQL Table

I checked the tables using:

```sql
\dt
```

Output:

```text
           List of relations
 Schema |   Name    | Type  |  Owner
--------+-----------+-------+----------
 public | employees | table | postgres
```

This confirmed that the `employees` table had been successfully restored.

---

## Step 5 — Verify PostgreSQL Data

I ran:

```sql
SELECT * FROM employees;
```

Output:

```text
 id |     name     |        position        | salary | department
----+--------------+------------------------+--------+------------
  1 | Ali Khan     | Software Engineer      |  75000 | Software
  2 | Sara Ahmed   | DevOps Engineer        |  90000 | DevOps
  3 | Ahmed Raza   | Database Administrator |  85000 | Database
  4 | Ayesha Malik | Project Manager        | 100000 | Management
```

The restored PostgreSQL data matched the data contained in the PostgreSQL dump.

---

# Important Commands Learned

## MySQL

Create restore database:

```bash
sudo mysql
```

```sql
CREATE DATABASE test_db_restore;
```

Restore database:

```bash
sudo mysql test_db_restore < ~/test_db_dump.sql
```

Verify tables:

```sql
USE test_db_restore;
SHOW TABLES;
```

Verify data:

```sql
SELECT * FROM employees;
```

---

## PostgreSQL

Create restore database:

```bash
sudo -u postgres psql
```

```sql
CREATE DATABASE test_db_restore;
```

Restore database:

```bash
sudo -u postgres psql test_db_restore < ~/postgres_test_db_dump.sql
```

Connect to restored database:

```sql
\c test_db_restore
```

Verify tables:

```sql
\dt
```

Verify data:

```sql
SELECT * FROM employees;
```

---

# Backup and Restore Flow

## MySQL

```text
test_db
   │
   │ mysqldump
   ▼
test_db_dump.sql
   │
   │ mysql restore
   ▼
test_db_restore
   │
   │ SELECT
   ▼
Restored employees data
```

## PostgreSQL

```text
test_db
   │
   │ pg_dump
   ▼
postgres_test_db_dump.sql
   │
   │ psql restore
   ▼
test_db_restore
   │
   │ SELECT
   ▼
Restored employees data
```

---

# Key Concepts Learned

### Database Restore

Database restoration is the process of taking a previously created database backup or dump and using it to recreate the database structure and data.

### Why Restore to a Separate Database?

For this practical task, I restored the dumps into:

```text
test_db_restore
```

instead of overwriting the original `test_db`.

This allowed me to:

* Preserve the original database
* Safely test the backup
* Verify that the dump was usable
* Compare the restored data with the original data

### Verification

A restore should not be considered successful only because the restore command completed.

I verified the restoration by checking:

```sql
SHOW TABLES;
```

for MySQL and:

```sql
\dt
```

for PostgreSQL.

I then verified the actual records using:

```sql
SELECT * FROM employees;
```

---

# MySQL vs PostgreSQL Restore

| Feature          | MySQL              | PostgreSQL                  |
| ---------------- | ------------------ | --------------------------- |
| Dump Tool        | `mysqldump`        | `pg_dump`                   |
| Restore Tool     | `mysql`            | `psql`                      |
| Dump File        | `test_db_dump.sql` | `postgres_test_db_dump.sql` |
| Restore Database | `test_db_restore`  | `test_db_restore`           |
| Table Restored   | `employees`        | `employees`                 |
| Data Verified    | Yes                | Yes                         |

---

# Result

Successfully restored the `test_db` database dump into separate restore databases for both MySQL and PostgreSQL.

### MySQL

```text
test_db_dump.sql
        ↓
test_db_restore
        ↓
employees table
        ↓
SELECT * FROM employees
        ↓
Data verified successfully
```

### PostgreSQL

```text
postgres_test_db_dump.sql
        ↓
test_db_restore
        ↓
employees table
        ↓
SELECT * FROM employees
        ↓
Data verified successfully
```
