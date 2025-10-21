# Using a Linked Server in SQL Server 2019 — Guide & Commands

This README explains how to query a remote SQL Server via a linked server entry (example name shown below).
You can paste these commands into `mssqlclient.py`, `sqlcmd`, SSMS, or any T-SQL prompt.

**Example linked-server name (contains backslash):**

```
WIN-Q13O908QBPG\SQLEXPRESS
```

> **Important:** Because the linked-server name contains a backslash, always quote it with square brackets when used:
>
> ```sql
> [WIN-Q13O908QBPG\SQLEXPRESS]
> ```

---

## 1. Quick concepts

* **Four-part name**: `[linked_server].[catalog].[schema].[object]` — simple but sometimes causes local-side processing.
* **OPENQUERY**: Sends the query to the remote server to execute there (recommended for enumeration / heavy queries).
* **EXECUTE AT**: Runs a T-SQL string on the remote server (useful for dynamic SQL and non-SELECT operations).
* **Permissions**: The queries run using the credentials configured for the linked server. If you see permission errors, the login mapping or remote permissions are the likely cause.

---

## 2. List linked servers on this instance

```sql
EXEC sp_linkedservers;
-- or
SELECT * FROM sys.servers WHERE is_linked = 1;
```

---

## 3. Enumerate databases on the linked server

### a) Four-part-name (simple)

```sql
SELECT name
FROM [WIN-Q13O908QBPG\SQLEXPRESS].master.sys.databases;
```

### b) OPENQUERY (recommended)

```sql
SELECT *
FROM OPENQUERY(
  [WIN-Q13O908QBPG\SQLEXPRESS],
  'SELECT name, database_id, state_desc FROM sys.databases'
);
```

### c) EXECUTE AT

```sql
EXEC ('SELECT name FROM sys.databases') AT [WIN-Q13O908QBPG\SQLEXPRESS];
```

---

## 4. Enumerate tables / schemas on the linked server

### a) Use `sp_tables_ex` (local proc that queries remote)

```sql
EXEC sp_tables_ex @table_server = 'WIN-Q13O908QBPG\SQLEXPRESS';
```

### b) Using OPENQUERY + INFORMATION_SCHEMA

```sql
SELECT TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME
FROM OPENQUERY(
  [WIN-Q13O908QBPG\SQLEXPRESS],
  'SELECT TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME
   FROM INFORMATION_SCHEMA.TABLES'
);
```

---

## 5. Enumerate logins, users, roles

### a) SQL logins (remote master)

```sql
SELECT name, create_date, modify_date, type_desc
FROM OPENQUERY(
  [WIN-Q13O908QBPG\SQLEXPRESS],
  'SELECT name, create_date, modify_date, type_desc FROM master.sys.sql_logins'
);
```

### b) Database users for a particular database (example pattern)

1. Get list of databases (see section 3).
2. For each database, run:

```sql
SELECT name, type_desc
FROM OPENQUERY(
  [WIN-Q13O908QBPG\SQLEXPRESS],
  'SELECT name, type_desc FROM [<DBNAME>].sys.database_principals'
);
```

(You can automate the loop with dynamic SQL + `EXECUTE AT` if needed.)

---

## 6. Troubleshooting & caveats

* **Permission errors**: Usually indicate the linked-server login mapping or remote permissions. Check how the linked server maps local credentials to remote credentials (security settings).
* **Performance**: For large result sets, prefer `OPENQUERY` or `EXECUTE AT` so most processing happens on the remote side.
* **Provider differences**: Behavior can vary by OLE DB provider; SQL Server linked servers to non-SQL providers may not support all constructs.
* **Four-part names**: Simpler, but may cause the local server to pull data and process it locally (less efficient for heavy queries).

---

## 7. Example `mssqlclient.py` session (copy-paste)

```
mssql> EXEC sp_linkedservers;
mssql> SELECT * FROM sys.servers WHERE is_linked = 1;
mssql> SELECT name FROM [WIN-Q13O908QBPG\SQLEXPRESS].master.sys.databases;
mssql> SELECT * FROM OPENQUERY([WIN-Q13O908QBPG\SQLEXPRESS], 'SELECT name, state_desc FROM sys.databases');
mssql> EXEC('SELECT name FROM sys.databases') AT [WIN-Q13O908QBPG\SQLEXPRESS];
mssql> EXEC sp_tables_ex @table_server = 'WIN-Q13O908QBPG\SQLEXPRESS';
```

---

## 8. Useful checklist to run now

* [ ] Run `EXEC sp_linkedservers;` to confirm the linked server exists.
* [ ] Run `SELECT * FROM OPENQUERY([<linked>], 'SELECT name FROM sys.databases');` to list remote DBs.
* [ ] If you get permission errors, record the exact message and check linked-server security mapping.
* [ ] Use `OPENQUERY` or `EXECUTE AT` for heavier enumeration queries.

---

## 9. Further reading / references

* Microsoft docs: Linked Servers, OPENQUERY, and `EXECUTE AT` — search Microsoft Docs for **Linked Servers (Database Engine)** and **OPENQUERY** for authoritative syntax and examples.
* Penetration-testing / enumeration cheat-sheets: look up MSSQL enumeration cheat sheets (many reputable sources collect common T-SQL enumeration commands for DBs and linked servers).

---

## 10. Notes / placeholders

If you want, replace every occurrence of `WIN-Q13O908QBPG\SQLEXPRESS` with a variable or the actual linked-server name for your environment.
If you want a single-file `.md` version with more examples (database → tables → columns → users automation), tell me and I’ll expand this file.
