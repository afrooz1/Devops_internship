**Task 4 — Database Dump**

````markdown
# Task 4 — Database Dump

## Objective

Practice creating logical database backups using the command-line dump utilities provided by MySQL and PostgreSQL.

In this task, I practiced:

- Creating a MySQL database dump using `mysqldump`
- Creating a PostgreSQL database dump using `pg_dump`
- Saving database dumps into SQL files
- Verifying that the dump files were created successfully
- Checking the dump files for table structure and data
- Understanding how database dumps can be used for backup and recovery

---

# Part 1 — MySQL Database Dump

## Step 1 — Attempted MySQL Dump

Initially, I tried to create the dump using:

```bash
mysqldump -u root -p test_db > test_db_dump.sql
````

The command returned:

```text
mysqldump: Got error: 1698:
Access denied for user 'root'@'localhost'
```

### Reason

The MySQL `root` user was configured with the `auth_socket` authentication plugin.

I verified this using:

```bash
sudo mysql
```

and:

```sql
SELECT user, host, plugin FROM mysql.user;
```

The result showed:

```text
root | localhost | auth_socket
```

This means the local MySQL root account authenticates through the operating-system user rather than a MySQL password.

---

## Step 2 — Create the MySQL Dump

Since the MySQL root account uses socket authentication, I created the dump using:

```bash
sudo mysqldump test_db > test_db_dump.sql
```

The command completed successfully without errors.

---

## Step 3 — Verify the MySQL Dump

I checked the files in my home directory:

```bash
ls
```

The dump file was present:

```text
test_db_dump.sql
```

# Part 2 — PostgreSQL Database Dump

## Step 1 — Create PostgreSQL Dump

I created the PostgreSQL dump using:

```bash
sudo -u postgres pg_dump test_db > postgres_test_db_dump.sql
```

The command completed successfully.

The PostgreSQL dump file was created:

```text
postgres_test_db_dump.sql
```

---

## Step 2 — Verify PostgreSQL Dump

I checked the file:

```bash
ls
```

The file was present:

```text
postgres_test_db_dump.sql
```

I verified that the `employees` table was included:

```bash
grep -n "employees" postgres_test_db_dump.sql
```

The output showed entries for:

```text
CREATE TABLE public.employees
```

```text
Data for Name: employees
```

```text
COPY public.employees
```

```text
employees_pkey
```

This confirmed that the dump contained the table structure, data, sequence, primary key, and permissions.

---

## Step 3 — Check Dump File Size

I checked the PostgreSQL dump size using:

```bash
du -h postgres_test_db_dump.sql
```

Output:

```text
4.0K    postgres_test_db_dump.sql
```

The small size is expected because the database currently contains only a small amount of data.

---

# Dump Files

After completing the task, I created the following dump files:

```text
test_db_dump.sql
postgres_test_db_dump.sql
```

### MySQL

```text
test_db_dump.sql
```

Created using:

```bash
sudo mysqldump test_db > test_db_dump.sql
```

### PostgreSQL

```text
postgres_test_db_dump.sql
```

Created using:

```bash
sudo -u postgres pg_dump test_db > postgres_test_db_dump.sql
```

---

# Important Commands Learned

## MySQL

Create a database dump:

```bash
mysqldump database_name > backup.sql
```

Create a dump using local root authentication:

```bash
sudo mysqldump database_name > backup.sql
```

Inspect a dump:

```bash
cat backup.sql
```

---

## PostgreSQL

Create a database dump:

```bash
pg_dump database_name > backup.sql
```

Create a dump using the PostgreSQL system user:

```bash
sudo -u postgres pg_dump database_name > backup.sql
```

Check the dump for a particular table:

```bash
grep -n "employees" backup.sql
```

Check file size:

```bash
du -h backup.sql
```

---

# Key Concepts Learned

### What is a Database Dump?

A database dump is a file containing information required to recreate a database or its objects and data.

A dump can contain:

* Table definitions
* Columns
* Constraints
* Primary keys
* Sequences
* Table data
* Permissions
* Other database objects

---

### `mysqldump`

`mysqldump` is the command-line utility used to create logical backups of MySQL databases.

Example:

```bash
sudo mysqldump test_db > test_db_dump.sql
```

---

### `pg_dump`

`pg_dump` is the PostgreSQL command-line utility used to create logical backups of PostgreSQL databases.

Example:

```bash
sudo -u postgres pg_dump test_db > postgres_test_db_dump.sql
```

---

### SQL Dump Files

The dump files contain SQL/database commands that can later be used during database restoration.

For example, the MySQL dump contained:

```sql
CREATE TABLE `employees` (...);
```

and:

```sql
INSERT INTO `employees` VALUES (...);
```

The PostgreSQL dump contained:

```sql
CREATE TABLE public.employees (...);
```

and:

```text
COPY public.employees
```

---

# MySQL vs PostgreSQL Dump

| Feature            | MySQL              | PostgreSQL                  |
| ------------------ | ------------------ | --------------------------- |
| Dump Utility       | `mysqldump`        | `pg_dump`                   |
| Database           | `test_db`          | `test_db`                   |
| Dump Format        | SQL                | SQL                         |
| Output File        | `test_db_dump.sql` | `postgres_test_db_dump.sql` |
| Table Included     | `employees`        | `employees`                 |
| Data Included      | Yes                | Yes                         |
| Structure Included | Yes                | Yes                         |

---

# Result

Successfully created and verified database dumps for both MySQL and PostgreSQL.

### MySQL

```text
test_db
   ↓
mysqldump
   ↓
test_db_dump.sql
```

### PostgreSQL

```text
test_db
   ↓
pg_dump
   ↓
postgres_test_db_dump.sql
```

Both dump files were successfully created and verified.

