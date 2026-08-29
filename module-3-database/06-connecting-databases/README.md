# Task 6 — Connecting to Databases

## Objective

The objective of this task was to practice connecting to MySQL and PostgreSQL databases locally and remotely.

In this task, I practiced:

* Connecting to MySQL using the command line.
* Connecting to PostgreSQL using the command line.
* Connecting to databases using an IP address and port.
* Configuring MySQL to accept remote connections.
* Configuring PostgreSQL to accept remote connections.
* Understanding database authentication and access configuration.
* Troubleshooting database connection errors.

---


# Part 1 — MySQL Local Connection

First, I connected to MySQL using the command line:

```bash
sudo mysql
```

Then I selected the database:

```sql
USE test_db;
```

I verified the employees table:

```sql
SELECT * FROM employees;
```

The database connection was successful.

---

# Part 2 — PostgreSQL Local Connection

I connected to PostgreSQL using:

```bash
sudo -u postgres psql
```

Then connected to the database:

```sql
\c test_db
```

I verified the available tables:

```sql
\dt
```

And checked the employee records:

```sql
SELECT * FROM employees;
```

The PostgreSQL local connection was successful.

---

# Part 3 — Find System IP Address

I checked the system IP address using:

```bash
hostname -I
```

Output:

```text
172.19.176.138
```

This IP was used to test remote database connections.

---

# Part 4 — Configure MySQL for Remote Connections

Initially, MySQL was listening only on localhost.

I checked the current configuration:

```bash
sudo mysql -e "SHOW VARIABLES LIKE 'bind_address';"
```

Initial result:

```text
127.0.0.1
```

This means MySQL was accepting connections only from the local machine.

I opened the MySQL configuration file:

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

I changed:

```ini
bind-address = 127.0.0.1
```

to:

```ini
bind-address = 0.0.0.0
```

Then restarted MySQL:

```bash
sudo systemctl restart mysql
```

I verified the setting:

```bash
sudo mysql -e "SHOW VARIABLES LIKE 'bind_address';"
```

Result:

```text
0.0.0.0
```

---

# Part 5 — Verify MySQL Listening Port

I checked whether MySQL was listening on port `3306`:

```bash
sudo ss -ltnp | grep 3306
```

The server was listening on port `3306`.

I then tested the connection using the system IP address.

During the process, I encountered:

```text
Host 'LAPTOP.mshome.net' is not allowed to connect
```

This showed that the MySQL server was reachable, but the user did not have permission to connect from that host.

I also encountered:

```text
Public Key Retrieval is not allowed
```

This was related to the MySQL authentication configuration used by the client.

After allowing the required connection, the MySQL connection was successfully established.

---

# Part 6 — Configure PostgreSQL for Remote Connections

Initially, PostgreSQL was listening only on localhost.

I checked the configuration:

```bash
sudo -u postgres psql -c "SHOW listen_addresses;"
```

Initial result:

```text
localhost
```

I opened the PostgreSQL configuration file:

```bash
sudo nano /etc/postgresql/16/main/postgresql.conf
```

I changed the setting to:

```ini
listen_addresses = '*'
```

Then restarted PostgreSQL:

```bash
sudo systemctl restart postgresql
```

I verified the configuration:

```bash
sudo -u postgres psql -c "SHOW listen_addresses;"
```

Result:

```text
*
```

I also verified that PostgreSQL was listening on port `5432`:

```bash
sudo ss -ltnp | grep 5432
```

The server was listening on:

```text
0.0.0.0:5432
```

---

# Part 7 — Configure PostgreSQL Client Authentication

For remote access, PostgreSQL also requires an appropriate rule in `pg_hba.conf`.

I opened:

```bash
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

I added:

```text
host    test_db    db_user    172.19.176.0/24    scram-sha-256
```

This allows `db_user` to connect to `test_db` from the `172.19.176.x` network using password authentication.

### Important Troubleshooting

Initially, I accidentally placed this rule inside:

```text
postgresql.conf
```

This caused PostgreSQL to fail with:

```text
invalid line in /etc/postgresql/16/main/postgresql.conf
```

The mistake was fixed by removing the rule from `postgresql.conf` and placing it correctly inside:

```text
pg_hba.conf
```

This taught me the difference between the two configuration files:

### `postgresql.conf`

Controls PostgreSQL server settings such as:

```ini
listen_addresses = '*'
port = 5432
```

### `pg_hba.conf`

Controls which users and hosts are allowed to connect:

```text
host    test_db    db_user    172.19.176.0/24    scram-sha-256
```

---

# Part 8 — Test PostgreSQL Remote Connection

I tested the PostgreSQL connection using:

```bash
psql -U db_user -d test_db -h 172.19.176.138
```

After entering the password, the connection was established successfully.

I then verified the data:

```sql
SELECT * FROM employees;
```

The employee records were displayed successfully.

---

# Part 9 — Database Connection Troubleshooting

During this task, I encountered and resolved several common database connection problems.

### Error 1 — Connection Refused

```text
Connection to 172.19.176.138:5432 refused
```

**Cause:** PostgreSQL was not running/listening because of an invalid configuration entry.

**Solution:** Corrected the configuration and restarted PostgreSQL.

---

### Error 2 — No `pg_hba.conf` Entry

```text
FATAL: no pg_hba.conf entry for host ...
```

**Cause:** PostgreSQL was reachable, but the connecting host was not allowed.

**Solution:** Added the appropriate rule to `pg_hba.conf`.

---

### Error 3 — Host Not Allowed in MySQL

```text
Host 'LAPTOP.mshome.net' is not allowed to connect
```

**Cause:** MySQL was reachable, but the user was not permitted to connect from that host.

**Solution:** Configured MySQL to accept the required network connection and allowed the user.

---

### Error 4 — Public Key Retrieval

```text
Public Key Retrieval is not allowed
```

**Cause:** MySQL client authentication configuration.

**Solution:** Allowed the required authentication/connection option in the client configuration.

---

# Useful Commands

### MySQL

Check MySQL port:

```bash
sudo mysql -e "SHOW VARIABLES LIKE 'port';"
```

Check MySQL bind address:

```bash
sudo mysql -e "SHOW VARIABLES LIKE 'bind_address';"
```

Check MySQL listening port:

```bash
sudo ss -ltnp | grep 3306
```

---

### PostgreSQL

Check PostgreSQL port:

```bash
sudo -u postgres psql -c "SHOW port;"
```

Check listening addresses:

```bash
sudo -u postgres psql -c "SHOW listen_addresses;"
```

Check PostgreSQL listening port:

```bash
sudo ss -ltnp | grep 5432
```

Check PostgreSQL cluster:

```bash
sudo pg_lsclusters
```

---

# What I Learned

Through this task, I learned:

* How to connect to MySQL using the command line.
* How to connect to PostgreSQL using `psql`.
* How database ports work.
* How to find the system IP address using `hostname -I`.
* How MySQL's `bind-address` controls network listening.
* How PostgreSQL's `listen_addresses` controls network listening.
* How `pg_hba.conf` controls PostgreSQL client authentication.
* How to connect to a database using an IP address.
* How to troubleshoot `Connection refused` errors.
* How to troubleshoot authentication and authorization errors.
* The difference between server configuration and authentication configuration.
* Why remote database access requires both network listening and user authorization.

---

# Conclusion

In this task, I successfully practiced **local and remote database connectivity** for both MySQL and PostgreSQL.

I configured MySQL to listen for network connections using:

```ini
bind-address = 0.0.0.0
```

and configured PostgreSQL using:

```ini
listen_addresses = '*'
```

I also configured PostgreSQL's `pg_hba.conf` to allow the required user and network.

The practical troubleshooting helped me understand that a successful remote database connection requires:

1. The database server must be running.
2. The server must listen on the required IP address and port.
3. The database user must have the required permissions.
4. The client host must be allowed by the database authentication configuration.
5. Firewall/network settings must allow the connection.

