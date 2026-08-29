# Task 3 — User Management

## Objective

Practice database user management and access control in **MySQL** and **PostgreSQL**.

In this task, I practiced:

* Creating a database user.
* Granting database and table-level permissions.
* Testing user permissions.
* Revoking a specific permission.
* Verifying that the revoked operation is denied.

---


# Part 1 — MySQL User Management

## 1. Check Existing Users

First, I checked the existing MySQL users:

```sql
SELECT User, Host FROM mysql.user;
```

The `db_user` account did not exist.

---

## 2. Create MySQL User

Created a new MySQL user named `db_user` with a password:

```sql
CREATE USER 'db_user'@'localhost'
IDENTIFIED BY 'DbUser@2026#Secure';
```

Verified the user:

```sql
SELECT User, Host
FROM mysql.user
WHERE User = 'db_user';
```

Output confirmed:

```text
+---------+-----------+
| User    | Host      |
+---------+-----------+
| db_user | localhost |
+---------+-----------+
```

---

## 3. Grant Permissions

Granted the following permissions on the `employees` table:

* SELECT
* INSERT
* UPDATE
* DELETE

```sql
GRANT SELECT, INSERT, UPDATE, DELETE
ON test_db.employees
TO 'db_user'@'localhost';
```

Verified the privileges:

```sql
SHOW GRANTS FOR 'db_user'@'localhost';
```

The user had:

```text
GRANT SELECT, INSERT, UPDATE, DELETE
ON test_db.employees
TO 'db_user'@'localhost'
```

---

## 4. Test MySQL Permissions

Logged in as `db_user`:

```bash
mysql -u db_user -p
```

Selected the database:

```sql
USE test_db;
```

### SELECT

```sql
SELECT * FROM employees;
```

The query executed successfully.

### INSERT

```sql
INSERT INTO employees
(name, position, salary, department)
VALUES
('Test User', 'Test Engineer', 60000, 'Testing');
```

The record was successfully inserted.

### UPDATE

```sql
UPDATE employees
SET salary = 65000
WHERE name = 'Test User';
```

The record was successfully updated.

### DELETE

```sql
DELETE FROM employees
WHERE name = 'Test User';
```

The record was successfully deleted.

---

## 5. Revoke INSERT Permission

Revoked only the `INSERT` privilege:

```sql
REVOKE INSERT
ON test_db.employees
FROM 'db_user'@'localhost';
```

Verified the permissions:

```sql
SHOW GRANTS FOR 'db_user'@'localhost';
```

The remaining privileges were:

```text
SELECT, UPDATE, DELETE
```

---

## 6. Verify Revoked Permission

Logged in again as `db_user` and attempted:

```sql
USE test_db;

INSERT INTO employees
(name, position, salary, department)
VALUES
('Blocked User', 'Test Engineer', 60000, 'Testing');
```

MySQL rejected the operation:

```text
ERROR 1142 (42000):
INSERT command denied to user 'db_user'@'localhost'
for table 'employees'
```

This confirmed that the `INSERT` privilege had been successfully revoked.

---

# Part 2 — PostgreSQL User Management

## 1. Check Existing Roles

Connected to PostgreSQL:

```bash
sudo -u postgres psql
```

Checked existing roles:

```sql
\du
```

The `db_user` role did not exist.

---

## 2. Create PostgreSQL User

Created the PostgreSQL user:

```sql
CREATE USER db_user
WITH PASSWORD 'DbUser@2026#Secure';
```

Verified the role:

```sql
\du
```

The `db_user` role was successfully created.

---

## 3. Grant Database Access

Granted permission to connect to the `test_db` database:

```sql
GRANT CONNECT
ON DATABASE test_db
TO db_user;
```

Connected to the database:

```sql
\c test_db
```

Verified that the `employees` table existed:

```sql
\dt
```

---

## 4. Grant Table Permissions

Granted the required permissions on the `employees` table:

```sql
GRANT SELECT, INSERT, UPDATE, DELETE
ON TABLE employees
TO db_user;
```

Verified the permissions:

```sql
\dp employees
```

The output showed:

```text
db_user=arwd/postgres
```

PostgreSQL privilege notation:

```text
r = SELECT
a = INSERT
w = UPDATE
d = DELETE
```

Therefore:

```text
arwd
```

means the user has:

* `r` → SELECT
* `a` → INSERT
* `w` → UPDATE
* `d` → DELETE

---

## 5. Test PostgreSQL Permissions

Connected to PostgreSQL as `db_user`:

```bash
psql -U db_user -d test_db -h localhost
```

### SELECT

```sql
SELECT * FROM employees;
```

Successfully retrieved employee records.

### INSERT

```sql
INSERT INTO employees
(name, position, salary, department)
VALUES
('Test User', 'Test Engineer', 60000, 'Testing');
```

Successfully inserted the test employee.

### UPDATE

```sql
UPDATE employees
SET salary = 65000
WHERE name = 'Test User';
```

Successfully updated the employee's salary.

### DELETE

```sql
DELETE FROM employees
WHERE name = 'Test User';
```

Successfully deleted the test employee.

---

## 6. Revoke INSERT Permission

Connected as the PostgreSQL administrator and revoked the `INSERT` privilege:

```sql
REVOKE INSERT
ON TABLE employees
FROM db_user;
```

Verified the permissions:

```sql
\dp employees
```

The privileges changed from:

```text
db_user=arwd/postgres
```

to:

```text
db_user=rwd/postgres
```

The `a` privilege, representing `INSERT`, was removed.

---

## 7. Verify Revoked Permission

Logged in as `db_user` and attempted to insert a new employee:

```sql
INSERT INTO employees
(name, position, salary, department)
VALUES
('Blocked User', 'Test Engineer', 60000, 'Testing');
```

PostgreSQL rejected the operation with a permission-denied error.

This confirmed that the `INSERT` privilege had been successfully revoked while the other permissions remained available.

---

# Permission Summary

| Database                  | SELECT | INSERT | UPDATE | DELETE |
| ------------------------- | -----: | -----: | -----: | -----: |
| MySQL — Initially         |      ✅ |      ✅ |      ✅ |      ✅ |
| MySQL — After Revoke      |      ✅ |      ❌ |      ✅ |      ✅ |
| PostgreSQL — Initially    |      ✅ |      ✅ |      ✅ |      ✅ |
| PostgreSQL — After Revoke |      ✅ |      ❌ |      ✅ |      ✅ |

---

# Key Learnings

### 1. Users should not automatically receive full database access

Specific permissions can be granted according to what a user actually needs.

### 2. Permissions can be granted individually

For example:

```sql
GRANT SELECT, INSERT, UPDATE, DELETE
ON ...
TO db_user;
```

### 3. Permissions can also be revoked individually

For example:

```sql
REVOKE INSERT
ON ...
FROM db_user;
```

This removes only the `INSERT` permission without removing the other permissions.

### 4. MySQL and PostgreSQL use different privilege systems

MySQL identifies the account using:

```text
'user'@'host'
```

while PostgreSQL uses **roles/users**.

PostgreSQL also represents table privileges using letters such as:

```text
r = SELECT
a = INSERT
w = UPDATE
d = DELETE
```

---

# Task Result

The **User Management** task was completed successfully in both **MySQL and PostgreSQL**.

I created `db_user`, granted the required permissions, tested the permissions through actual database operations, revoked `INSERT`, and verified that `INSERT` was subsequently denied while the remaining permissions continued to work.
