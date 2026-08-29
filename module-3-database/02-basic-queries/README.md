# Task 2 — Basic Queries

## Objective

Practice fundamental SQL queries for retrieving, filtering, sorting, and grouping data using **MySQL** and **PostgreSQL**.

In this task, I practiced:

* Retrieving all records using `SELECT`
* Selecting specific columns
* Filtering records using `WHERE`
* Sorting records using `ORDER BY`
* Grouping records using `GROUP BY`
* Counting records using `COUNT()`
* Understanding SQL aliases
* Comparing query behavior between MySQL and PostgreSQL

---


# Part 1 — Preparing Department Data

The `department` column was added during Task 1 but initially contained `NULL` values.

To practice `GROUP BY`, department values were assigned to the existing employees.

### PostgreSQL

```sql
UPDATE employees
SET department = 'Software'
WHERE id = 1;

UPDATE employees
SET department = 'DevOps'
WHERE id = 2;

UPDATE employees
SET department = 'Database'
WHERE id = 3;

UPDATE employees
SET department = 'Management'
WHERE id = 4;
```

### MySQL

```sql
UPDATE employees
SET department = 'DevOps'
WHERE id = 2;

UPDATE employees
SET department = 'Database'
WHERE id = 3;

UPDATE employees
SET department = 'Management'
WHERE id = 4;

UPDATE employees
SET department = 'IT'
WHERE id = 5;
```

The difference occurred because employee `id = 1` had already been deleted from MySQL during Task 1, while employee `id = 5` had been deleted from PostgreSQL.

---

# Part 2 — MySQL

## 1. SELECT — Retrieve All Records

Used `SELECT *` to retrieve all columns and records from the `employees` table:

```sql
SELECT * FROM employees;
```

The `*` represents all columns.

---

## 2. WHERE — Filter Employees by Salary

Retrieved only the employee name and salary for employees earning more than `85000`:

```sql
SELECT name, salary
FROM employees
WHERE salary > 85000;
```

Result:

```text
Sara Ahmed   | 90000
Ayesha Malik | 100000
```

The `WHERE` clause filters records according to a specified condition.

The task example used `50000`, but `85000` was used for additional practice.

---

## 3. ORDER BY — Sort by Salary

Sorted employees by salary in descending order:

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

Result was ordered from the highest salary to the lowest:

```text
100000
90000
85000
70000
```

### Sorting Options

```sql
ORDER BY salary ASC;
```

Sorts from lowest to highest.

```sql
ORDER BY salary DESC;
```

Sorts from highest to lowest.

---

## 4. GROUP BY — Count Employees by Department

Used `GROUP BY` with `COUNT()`:

```sql
SELECT department, COUNT(*) AS employees_count
FROM employees
GROUP BY department;
```

Result:

```text
DevOps      | 1
Database    | 1
Management  | 1
IT          | 1
```

`GROUP BY` creates groups based on the department, while `COUNT(*)` counts the records in each group.

---

# Part 3 — PostgreSQL

## 1. SELECT — Retrieve All Records

Used:

```sql
SELECT * FROM employees;
```

This returned all columns and records from the PostgreSQL `employees` table.

---

## 2. WHERE — Filter Employees by Salary

Used:

```sql
SELECT name, salary
FROM employees
WHERE salary > 85000;
```

Result:

```text
Sara Ahmed   | 90000
Ayesha Malik | 100000
```

This demonstrated how `WHERE` filters records based on a condition.

---

## 3. ORDER BY — Sort by Salary

Used:

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

The result was sorted from the highest salary to the lowest:

```text
100000
90000
85000
75000
```

---

## 4. GROUP BY — Count Employees by Department

Used:

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

Result:

```text
DevOps      | 1
Database    | 1
Software    | 1
Management  | 1
```

This demonstrated grouping records by department and counting employees in each group.

---

# SQL Concepts Learned

## SELECT

Used to retrieve data from a table.

```sql
SELECT * FROM employees;
```

`*` means all columns.

Specific columns can also be selected:

```sql
SELECT name, salary
FROM employees;
```

---

## WHERE

Used to filter records:

```sql
SELECT name, salary
FROM employees
WHERE salary > 85000;
```

Only records satisfying the condition are returned.

---

## ORDER BY

Used to sort query results:

```sql
ORDER BY salary DESC;
```

* `ASC` → ascending
* `DESC` → descending

---

## GROUP BY

Used to group rows that have the same value:

```sql
GROUP BY department;
```

It is commonly used with aggregate functions such as:

```sql
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

## COUNT()

Used to count records:

```sql
COUNT(*)
```

For example:

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

counts the employees in each department.

---

## AS — Column Alias

Used an alias to give the calculated column a meaningful name:

```sql
COUNT(*) AS employee_count
```

Here, `employee_count` is the **name of the result column**, not a table.

For example, this is incorrect:

```sql
SELECT department, COUNT(*)
FROM employees_count
GROUP BY department;
```

because `employees_count` is not a table.

The correct query is:

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

---


# MySQL vs PostgreSQL

The main SQL queries used in this task were essentially the same in both database systems.

| Operation    | MySQL                      | PostgreSQL |
| ------------ | -------------------------- | ---------- |
| Retrieve all | `SELECT * FROM employees;` | Same       |
| Filter       | `WHERE salary > 85000`     | Same       |
| Sort         | `ORDER BY salary DESC`     | Same       |
| Group        | `GROUP BY department`      | Same       |
| Count        | `COUNT(*)`                 | Same       |
| Alias        | `AS employee_count`        | Same       |

This demonstrated that **standard SQL syntax is largely shared between MySQL and PostgreSQL**, even though their database administration and CLI commands can differ.

---

# Task Completion

* [x] Used `SELECT` to retrieve all employee records
* [x] Selected specific columns using `SELECT`
* [x] Used `WHERE` to filter employees by salary
* [x] Used `ORDER BY` to sort salaries in descending order
* [x] Used `GROUP BY` to group employees by department
* [x] Used `COUNT()` to count employees in each department
* [x] Used column aliases with `AS`
* [x] Practiced queries in MySQL
* [x] Practiced queries in PostgreSQL
* [x] Encountered and resolved SQL syntax and naming errors
* [x] Verified query results

## Conclusion

This task provided practical experience with basic SQL querying in **MySQL and PostgreSQL**. I practiced retrieving specific data, filtering records, sorting results, grouping records, and using aggregate functions.

