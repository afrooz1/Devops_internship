# Task 8 — Backup and Recovery Procedures

## Objective

Create a backup strategy for the `test_db` database and automate the backup process using a Bash script and Cron.

In this task, I practiced:

* Creating MySQL database backups using `mysqldump`.
* Creating PostgreSQL database backups using `pg_dump`.
* Storing database backups in a dedicated directory.
* Adding timestamps to backup filenames.
* Automating database backups using Cron.
* Testing the automated backup process.
* Restoring backups into a separate recovery database.
* Verifying that the restored data is correct.

---

## 1. Backup Strategy

For this task, the backup strategy is:

| Item                   | Configuration               |
| ---------------------- | --------------------------- |
| Database               | `test_db`                   |
| MySQL backup tool      | `mysqldump`                 |
| PostgreSQL backup tool | `pg_dump`                   |
| Backup location        | `~/database-backups`        |
| Backup format          | `.sql`                      |
| Filename               | Timestamp-based             |
| Automation             | Cron                        |
| Backup frequency       | Every minute during testing |
| Recovery database      | `test_db_recovery`          |

> **Note:** Cron was configured to run every minute during testing so that the automated backup could be verified immediately.

---

# 2. Create Backup Directory

Created a dedicated directory for database backups:

```bash
mkdir -p ~/database-backups
```

Verified the directory:

```bash
ls ~/database-backups
```

---

# 3. Backup Script

Created the script:

```bash
nano ~/database-backup.sh
```

The final script:

```bash
#!/bin/bash

BACKUP_DIR="$HOME/database-backups"
DATE=$(date +%Y-%m-%d_%H-%M-%S)

mkdir -p "$BACKUP_DIR"

sudo mysqldump test_db > "$BACKUP_DIR/mysql_test_db_$DATE.sql"

sudo -u postgres pg_dump test_db > "$BACKUP_DIR/postgres_test_db_$DATE.sql"

echo "Database backup completed: $DATE"
```

---

## 4. Script Explanation

### Bash interpreter

```bash
#!/bin/bash
```

Specifies that the script should be executed using Bash.

### Backup directory

```bash
BACKUP_DIR="$HOME/database-backups"
```

Stores the location where backup files will be created.

### Timestamp

```bash
DATE=$(date +%Y-%m-%d_%H-%M-%S)
```

Creates a timestamp such as:

```text
2026-08-24_14-47-06
```

This prevents new backups from overwriting previous backups.

### Create directory

```bash
mkdir -p "$BACKUP_DIR"
```

Creates the backup directory if it does not already exist.

### MySQL backup

```bash
sudo mysqldump test_db > "$BACKUP_DIR/mysql_test_db_$DATE.sql"
```

Creates an SQL dump of the MySQL `test_db` database.

### PostgreSQL backup

```bash
sudo -u postgres pg_dump test_db > "$BACKUP_DIR/postgres_test_db_$DATE.sql"
```

Creates an SQL dump of the PostgreSQL `test_db` database.

### Completion message

```bash
echo "Database backup completed: $DATE"
```

Displays a message showing when the backup was completed.

---

# 5. Make Script Executable

The script was made executable using:

```bash
chmod +x ~/database-backup.sh
```

The script was also tested manually.

---

# 6. Cron Automation

Cron was used to automatically execute the backup script.

Opened the user's crontab:

```bash
crontab -e
```

For testing, Cron was configured to run the script every minute:

```cron
* * * * * /home/afrooz/database-backup.sh
```

### Cron meaning

```text
* * * * *
│ │ │ │ │
│ │ │ │ └── Day of week
│ │ │ └──── Month
│ │ └────── Day of month
│ └──────── Hour
└────────── Minute
```

`* * * * *` means:

> Run the command every minute.

This was useful for testing because I did not have to wait until the next day to verify the automation.

---

# 7. Verify Automated Backups

After configuring Cron, the backup directory was checked:

```bash
ls ~/database-backups
```

The following timestamped backup files were generated:

```text
mysql_test_db_2026-08-24_14-38-05.sql
mysql_test_db_2026-08-24_14-39-05.sql
mysql_test_db_2026-08-24_14-46-03.sql
mysql_test_db_2026-08-24_14-47-06.sql

postgres_test_db_2026-08-24_14-38-05.sql
postgres_test_db_2026-08-24_14-39-05.sql
postgres_test_db_2026-08-24_14-46-03.sql
postgres_test_db_2026-08-24_14-47-06.sql
```

This confirmed that the Cron job successfully executed the backup script.

---

# 8. MySQL Recovery Test

Created a separate database for recovery:

```bash
sudo mysql -e "CREATE DATABASE test_db_recovery;"
```

Restored the MySQL backup:

```bash
sudo mysql test_db_recovery < ~/database-backups/mysql_test_db_backup.sql
```

Verified the restored table:

```bash
sudo mysql -e "SHOW TABLES FROM test_db_recovery;"
```

Result:

```text
employees
```

The restored data was then verified using:

```bash
sudo mysql -e "SELECT * FROM test_db_recovery.employees;"
```

The employee records were successfully restored.

---

# 9. PostgreSQL Recovery Test

Created the PostgreSQL recovery database:

```bash
sudo -u postgres psql -c "CREATE DATABASE test_db_recovery;"
```

Restored the PostgreSQL backup:

```bash
sudo -u postgres psql -d test_db_recovery < ~/database-backups/postgres_test_db_backup.sql
```

Verified the table:

```bash
sudo -u postgres psql -d test_db_recovery -c "\dt"
```

Result:

```text
 Schema |   Name    | Type  |  Owner
--------+-----------+-------+----------
 public | employees | table | postgres
```

The restored data was verified using:

```bash
sudo -u postgres psql -d test_db_recovery -c "SELECT * FROM employees;"
```

The employee records were successfully restored.

---

# 10. Backup Workflow

The complete workflow is:

```text
             Cron
               |
               | Every minute during testing
               ↓
       database-backup.sh
               |
       ┌───────┴────────┐
       ↓                ↓
    MySQL           PostgreSQL
   mysqldump          pg_dump
       ↓                ↓
       └───────┬────────┘
               ↓
       database-backups/
               |
               ↓
     Timestamped .sql files
               |
               ↓
        Recovery Database
               |
               ↓
       Verify Restored Data
```

---

# 11. Benefits of Automated Backups

Automating database backups provides several benefits:

* **Consistency** — Backups are created automatically.
* **Reduced human error** — No need to remember to run the backup manually.
* **Backup history** — Timestamped files keep multiple backup versions.
* **Disaster recovery** — A backup can be restored if the original database is damaged.
* **Easy verification** — Recovery can be tested using a separate database.

---

# 12. Conclusion

In this task, I implemented a basic database backup and recovery strategy for both **MySQL and PostgreSQL**.

I created Bash automation using `mysqldump` and `pg_dump`, stored timestamped backups in `~/database-backups`, and configured Cron to execute the backup script automatically.

I also tested the recovery process by restoring the backups into a separate `test_db_recovery` database and verifying the `employees` table and its data.

### Final Result

```text
MySQL Backup       → Successful
PostgreSQL Backup  → Successful
Bash Script        → Successful
Cron Automation    → Tested Successfully
MySQL Recovery     → Successful
PostgreSQL Recovery → Successful
Data Verification  → Successful
```

