# Database Module

This module covers the fundamentals of **MySQL and PostgreSQL** through practical, hands-on tasks.

## Objectives

In this module, I practiced:

* Database and table creation
* Basic SQL queries
* User management and permissions
* Database backup and restore
* Local and remote database connections
* Database port configuration
* Automated backup and recovery

## Tasks

| Task   | Topic                          |
| ------ | ------------------------------ |
| Task 1 | Database Basics                |
| Task 2 | Basic Queries                  |
| Task 3 | User Management                |
| Task 4 | Database Dump                  |
| Task 5 | Database Restore               |
| Task 6 | Connecting to Databases        |
| Task 7 | Port Settings                  |
| Task 8 | Backup and Recovery Procedures |

## Databases Used

### MySQL

* Default Port: `3306`
* Dump Tool: `mysqldump`
* CLI Client: `mysql`

### PostgreSQL

* Default Port: `5432`
* Dump Tool: `pg_dump`
* CLI Client: `psql`

## Main Concepts Practiced

* Databases and tables
* Primary keys and automatic IDs
* `SELECT`, `WHERE`, `ORDER BY`, `GROUP BY`
* `COUNT()` and column aliases
* Database users and roles
* `GRANT` and `REVOKE`
* Database dumps
* Database restoration
* Local and remote connections
* Database ports and configuration
* Bash scripting
* Cron automation
* Backup and recovery

## Backup & Recovery

A complete backup and recovery workflow was practiced for both databases:

```text
Database
   ↓
Backup
   ↓
SQL Dump
   ↓
Restore
   ↓
Recovery Database
   ↓
Verify Data
```

Automated backups were also configured using a Bash script and Cron.

## Key Learning

This module provided practical experience working with relational databases and helped me understand how **MySQL and PostgreSQL** are managed in a Linux/DevOps environment.

I also practiced troubleshooting database permissions, connections, ports, backups, and recovery procedures.

## Completion

*  Database Basics
*  Basic SQL Queries
*  User Management
*  Database Dump
*  Database Restore
*  Database Connections
*  Port Configuration
*  Backup & Recovery
