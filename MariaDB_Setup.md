# MariaDB Setup

This project uses the following database configuration on every machine:

| Setting | Value |
|---|---|
| Database | `tms` |
| Username | `tms` |
| Password | `hello_world` |
| Host | `localhost` |
| Port | `3306` |

## Linux / Debian

Open MariaDB:

```bash
sudo mariadb
```

Create the database:

```sql
CREATE DATABASE tms;
```

Configure the `tms` user:

```sql
ALTER USER 'tms'@'localhost' IDENTIFIED BY 'hello_world';
```

Grant access to the project database:

```sql
GRANT ALL PRIVILEGES ON tms.* TO 'tms'@'localhost';
```

Allow Prisma migrations to create the temporary shadow database:

```sql
GRANT CREATE ON *.* TO 'tms'@'localhost';
```

Apply the changes:

```sql
FLUSH PRIVILEGES;
```

Verify the permissions:

```sql
SHOW GRANTS FOR 'tms'@'localhost';
```

Exit MariaDB:

```sql
EXIT;
```

---

## Windows + XAMPP

Start **MySQL/MariaDB** from the XAMPP Control Panel.

Open phpMyAdmin:

```text
http://localhost/phpmyadmin
```

Go to the **SQL** tab.

Create the database:

```sql
CREATE DATABASE tms;
```

Create the application user:

```sql
CREATE USER 'tms'@'localhost' IDENTIFIED BY 'hello_world';
```

Grant access to the project database:

```sql
GRANT ALL PRIVILEGES ON tms.* TO 'tms'@'localhost';
```

Allow Prisma migrations to create the temporary shadow database:

```sql
GRANT CREATE ON *.* TO 'tms'@'localhost';
```

Apply the changes:

```sql
FLUSH PRIVILEGES;
```

Verify the permissions:

```sql
SHOW GRANTS FOR 'tms'@'localhost';
```

---

## Final Database Configuration

Both Linux and Windows should have the same:

```text
Host:     localhost
Port:     3306
Database: tms
User:     tms
Password: hello_world
```

The resulting connection string is:

```text
mysql://tms:hello_world@localhost:3306/tms
```

> **Note:** MariaDB is compatible with Prisma's MySQL provider. The database server can be MariaDB while the connection uses the MySQL protocol/connection format.

### XAMPP Port

Make sure XAMPP's MariaDB/MySQL is running on port `3306`.

If XAMPP is configured to use another port, the port must be changed accordingly.