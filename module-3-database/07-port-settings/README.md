# Task 7 — Port Settings

## Objective

Check the current port settings for MySQL and PostgreSQL, change their default ports, restart the database services, and verify that both databases are running on the new ports.

### Default Ports

- MySQL: `3306`
- PostgreSQL: `5432`

### New Ports

- MySQL: `3307`
- PostgreSQL: `5433`

---


# Part 1 — MySQL Port Settings

## Step 1 — Check Current MySQL Port

The current MySQL port was checked using:

```bash
sudo mysql -e "SHOW VARIABLES LIKE 'port';"
```

Output:

```text
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| port          | 3306  |
+---------------+-------+
```

This confirmed that MySQL was running on the default port `3306`.

---

## Step 2 — Modify MySQL Configuration

The MySQL configuration file was opened using:

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

The following setting was added under the `[mysqld]` section:

```ini
port = 3307
```

The file was saved after making the change.

---

## Step 3 — Restart MySQL

MySQL was restarted using:

```bash
sudo systemctl restart mysql
```

---

## Step 4 — Verify MySQL Port

The port was checked again:

```bash
sudo mysql -e "SHOW VARIABLES LIKE 'port';"
```

Output:

```text
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| port          | 3307  |
+---------------+-------+
```

This confirmed that MySQL was successfully changed from port `3306` to `3307`.

---

## Step 5 — Verify MySQL Is Listening

The new MySQL port was checked using:

```bash
sudo ss -ltnp | grep 3307
```

Output:

```text
LISTEN 0 151 127.0.0.1:3307 0.0.0.0:* users:(("mysqld",...))
```

MySQL was successfully listening on port `3307`.

---

# Part 2 — PostgreSQL Port Settings

## Step 1 — Check Current PostgreSQL Port

The current PostgreSQL port was checked using:

```bash
sudo -u postgres psql -c "SHOW port;"
```

Output:

```text
 port
------
 5432
(1 row)
```

This confirmed that PostgreSQL was running on the default port `5432`.

---

## Step 2 — Find PostgreSQL Configuration File

The PostgreSQL configuration file was located using:

```bash
sudo -u postgres psql -c "SHOW config_file;"
```

Output:

```text
/etc/postgresql/16/main/postgresql.conf
```

---

## Step 3 — Modify PostgreSQL Configuration

The configuration file was opened using:

```bash
sudo nano /etc/postgresql/16/main/postgresql.conf
```

The port setting was changed to:

```ini
port = 5433
```

The file was saved after making the change.

---

## Step 4 — Restart PostgreSQL

PostgreSQL was restarted using:

```bash
sudo systemctl restart postgresql
```

---

## Step 5 — Verify PostgreSQL Port

The port was checked again:

```bash
sudo -u postgres psql -c "SHOW port;"
```

Output:

```text
 port
------
 5433
(1 row)
```

This confirmed that PostgreSQL was successfully changed from port `5432` to `5433`.

---

## Step 6 — Verify PostgreSQL Is Listening

The new PostgreSQL port was checked using:

```bash
sudo ss -ltnp | grep 5433
```

Output:

```text
LISTEN 0 200 127.0.0.1:5433 0.0.0.0:* users:(("postgres",...))
```

PostgreSQL was successfully listening on port `5433`.

---

# Final Configuration

| Database | Default Port | New Port | Status |
|----------|--------------|----------|--------|
| MySQL | 3306 | 3307 |  Running |
| PostgreSQL | 5432 | 5433 |  Running |

---

## What I Learned

- MySQL uses port `3306` by default.
- PostgreSQL uses port `5432` by default.
- Database ports can be changed through configuration files.
- MySQL server configuration is stored in `mysqld.cnf`.
- PostgreSQL configuration is stored in `postgresql.conf`.
- Database services need to be restarted after configuration changes.
- The `ss` command can be used to check listening ports.
- Applications and database clients must use the new port after changing it.

---

## Important Commands

### MySQL

Check port:

```bash
sudo mysql -e "SHOW VARIABLES LIKE 'port';"
```

Restart service:

```bash
sudo systemctl restart mysql
```

Verify listening port:

```bash
sudo ss -ltnp | grep 3307
```

### PostgreSQL

Check port:

```bash
sudo -u postgres psql -c "SHOW port;"
```

Restart service:

```bash
sudo systemctl restart postgresql
```

Verify listening port:

```bash
sudo ss -ltnp | grep 5433
```

---

## Result

Task 7 was successfully completed.

MySQL was changed from port `3306` to `3307`, and PostgreSQL was changed from port `5432` to `5433`.

Both database services were restarted and successfully verified as listening on their new ports.