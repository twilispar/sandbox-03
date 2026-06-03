> This page location: Extensions > dblink
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup and usage of the `dblink` extension in Neon, enabling connections to and querying of remote Postgres databases for data integration and cross-database operations.

# The dblink extension

Connect to and query other Postgres databases from Neon using dblink

The `dblink` extension provides the ability to connect to other Postgres databases from within your current database. This is invaluable for tasks such as data integration, cross-database querying, and building applications that span multiple database instances. `dblink` allows you to execute queries on these remote databases and retrieve the results directly into your Neon project.

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

This guide will walk you through the fundamentals of using the `dblink` extension in your Neon project. You'll learn how to enable the extension, establish connections to remote Postgres databases, execute queries against them, and retrieve the results. We'll explore different connection methods and discuss important considerations for using `dblink` effectively.

**Note:** `dblink` is a core Postgres extension and can be enabled on any Neon project. It allows direct connections to other Postgres databases. For a more structured and potentially more secure way to access data in external data sources (including non-Postgres databases), consider using [Foreign Data Wrappers](https://neon.com/docs/extensions/postgres_fdw).

**Version availability:**

Please refer to the [list of all extensions](https://neon.com/docs/extensions/pg-extensions) available in Neon for up-to-date extension version information.

## Enable the `dblink` extension

You can enable the extension by running the following `CREATE EXTENSION` statement in the [Neon SQL Editor](https://neon.com/docs/get-started/query-with-neon-sql-editor) or from a client such as [psql](https://neon.com/docs/connect/query-with-psql-editor) that is connected to your Neon database.

```sql
CREATE EXTENSION IF NOT EXISTS dblink;
```

## Connecting to a remote database

The `dblink` extension provides the `dblink_connect` function to establish connections to remote Postgres databases. You can connect by providing the connection details directly in the function call or by using a named connection that you can reference in subsequent queries.

The most direct way to connect is by providing a connection string. This string includes all the necessary information to connect to the remote database.

### Named connections

To establish a named connection using `dblink_connect`, use the following syntax:

```sql
SELECT dblink_connect('my_remote_db', 'host=my_remote_host port=5432 dbname=my_remote_database user=my_remote_user password=my_remote_password sslmode=require&channel_binding=require');
```

In this example:

- `'my_remote_db'` is a name you assign to this connection for later use.
- The connection string specifies the host, port, database name, user, password, and SSL mode of the remote Postgres instance. **Replace these placeholders with your actual remote database credentials.**
- `sslmode=require` is recommended for security to ensure an encrypted connection.

You should receive a response like:

```text
 dblink_connect
----------------
 OK
(1 row)
```

### Unnamed connections

You can also connect without naming the connection. This is useful for one-off queries or when you don't need to reference the connection in subsequent queries.

```sql
SELECT dblink_connect('host=my_remote_host port=5432 dbname=my_remote_database user=my_remote_user password=my_remote_password sslmode=require&channel_binding=require');
```

**Tip: Did you know?**

Multiple named connections can be open at once, but only one unnamed connection is permitted at a time. The connection will persist until closed or until the database session is ended.

## Executing queries on the remote database

Once a connection is established, you can use the `dblink` function to execute queries on the remote database.

### With Named connections

```sql
SELECT *
FROM dblink('my_remote_db', 'SELECT table_name FROM information_schema.tables WHERE table_schema = ''public''')
AS remote_tables(table_name TEXT);
```

In this example:

- `'my_remote_db'` refers to the connection name established earlier.
- `'SELECT table_name FROM information_schema.tables WHERE table_schema = 'public''` is the SQL query you want to execute on the remote database.
- `AS remote_tables(table_name TEXT)` defines the structure of the returned data, specifying the column name (`table_name`) and its data type (`TEXT`). **This is crucial as `dblink` needs to know the expected structure of the results.**

You should receive a list of tables from the `public` schema of the remote database.

### With Unnamed connections

When using an unnamed connection, you can execute queries directly without referencing a named connection.

```sql
SELECT *
FROM dblink('host=my_remote_host port=5432 dbname=my_remote_database user=my_remote_user password=my_remote_password sslmode=require&channel_binding=require',
            'SELECT table_name FROM information_schema.tables WHERE table_schema = ''public''')
AS remote_tables(table_name TEXT);
```

## Retrieving data from the remote database

The results of the remote query are returned as a set of rows. You can use standard SQL to further process or integrate this data within your Neon database.

```sql
SELECT rt.table_name
FROM dblink('my_remote_db', 'SELECT table_name FROM information_schema.tables WHERE table_schema = ''public''')
AS rt(table_name TEXT)
WHERE rt.table_name LIKE 'user%';
```

This query retrieves the names of tables in the remote database that start with "user".

```sql
SELECT *
FROM dblink('my_remote_db', 'SELECT id, user_id, task, is_complete, inserted_at FROM todos')
AS rows(id int, user_id TEXT, task TEXT, is_complete BOOLEAN, inserted_at text);
```

This query retrieves the rows from a `todos` table in the remote database.

## Closing connections

It's good practice to close connections when you're finished with them to free up resources. Use the `dblink_disconnect` function.

```sql
SELECT dblink_disconnect('my_remote_db');
```

To disconnect from an unnamed connection, you can use the following:

```sql
SELECT dblink_disconnect();
```

## Using Named Connections for convenience

Naming your connections with `dblink_connect` can simplify your queries, especially if you frequently access the same remote database.

```sql
-- Connect with a name
SELECT dblink_connect('production_db', 'host=prod_host port=5432 dbname=prod_data user=reporter password=securepass sslmode=require&channel_binding=require');

-- Execute queries using the named connection
SELECT * FROM dblink('production_db', 'SELECT count(*) FROM orders') AS order_count(count int);

-- Disconnect
SELECT dblink_disconnect('production_db');
```

## Practical Examples

### Data Synchronization:

You can use `dblink` to periodically pull data from a remote database into your Neon project for reporting or analysis.

```sql
-- Using dblink to insert data from a remote table
INSERT INTO local_staging_table (col1, col2)
SELECT remote_col1, remote_col2
FROM dblink('remote_db', 'SELECT col1, col2 FROM remote_table')
AS rt(remote_col1 INTEGER, remote_col2 TEXT);
```

### Cross-Database reporting:

Generate reports that combine data from your Neon database and one or more remote Postgres databases.

```sql
SELECT l.customer_name, r.order_total
FROM customers l
JOIN dblink('orders_db', 'SELECT customer_id, sum(amount) AS order_total FROM orders GROUP BY customer_id')
AS r(customer_id INTEGER, order_total NUMERIC) ON l.customer_id = r.customer_id;
```

## Advanced `dblink` functions

The `dblink` extension provides additional functions to help manage and interact with remote databases:

- **`dblink_get_connections()`:** This function is helpful for monitoring and managing your `dblink` connections. It returns a list of the names of all currently open, named `dblink` connections in the current session. This can be useful for troubleshooting or ensuring connections are being managed correctly.

  **SQL**

  ```sql
  SELECT * FROM dblink_get_connections();
  ```

  **bash**

  ```bash
  dblink_get_connections
  ------------------
  {my_remote_db}
  ```

- **`dblink_error_message(TEXT connname)`:** When working with remote databases, errors can occur. This function allows you to retrieve the last error message associated with a specific named `dblink` connection. This is invaluable for debugging issues that arise during remote queries.

- **`dblink_send_query(TEXT connname, text sql)`:** This function sends a query to a named `dblink` connection without waiting for the result. This is useful for executing long-running queries on the remote database without blocking the current session. The return value is 1 if the query was successfully dispatched, or 0 otherwise.

- **`dblink_get_result(TEXT connname)`:** This function retrieves the result of a query that was previously sent using `dblink_send_query`. It returns the result set as a set of rows, allowing you to process the data as needed.

- **`dblink_cancel_query(TEXT connname)`:** This function tries to cancel the currently executing query on a named `dblink` connection. This can be useful if you need to stop a long-running query that is consuming resources on the remote database. The return value is 'OK' if the query was successfully canceled, or the error message as a text otherwise.

## Security considerations

- **Credentials:** Using `dblink` is inherently less secure than other methods of accessing remote data, as it requires storing credentials in the connection strings. For this reason, it may be preferable to use Foreign Data Wrappers or other secure methods.
- **Network Security:** Ensure that network access is properly configured to allow connections between your Neon project and the remote database server. Firewalls and security groups might need adjustments.
- **`sslmode`:** Always use `sslmode=require&channel_binding=require` in your connection strings to encrypt communication and ensure enhanced security against man-in-the-middle attacks.
- **Principle of Least Privilege:** Grant only the necessary permissions to the `dblink` connecting user on the remote database.

## Better alternatives: Foreign Data Wrappers

While `dblink` provides direct connectivity, Postgres' Foreign Data Wrappers (FDW) offer a more integrated and often more manageable approach for accessing external data. The `postgres_fdw` allows you to define a _foreign server_ and _foreign tables_ that represent tables in the remote database. You can learn more about FDWs in our [postgres_fdw](https://neon.com/docs/extensions/postgres_fdw) guide.

## Conclusion

`dblink` lets you connect to and query remote Postgres databases from your Neon project. It's flexible enough for one-off data pulls or complex cross-database queries. Keep security in mind when managing connections and credentials. For more structured access, see [postgres_fdw](https://neon.com/docs/extensions/postgres_fdw).

## Reference

- [PostgreSQL `dblink` Documentation](https://www.postgresql.org/docs/current/dblink.html)
- [PostgreSQL Foreign Data Wrappers](https://www.postgresql.org/docs/current/postgres-fdw.html)

---

## Related docs (Extensions)

- [Extension explorer](https://neon.com/docs/extensions/extension-explorer)
- [anon](https://neon.com/docs/extensions/postgresql-anonymizer)
- [btree_gin](https://neon.com/docs/extensions/btree_gin)
- [btree_gist](https://neon.com/docs/extensions/btree_gist)
- [citext](https://neon.com/docs/extensions/citext)
- [cube](https://neon.com/docs/extensions/cube)
- [dict_int](https://neon.com/docs/extensions/dict_int)
- [earthdistance](https://neon.com/docs/extensions/earthdistance)
- [fuzzystrmatch](https://neon.com/docs/extensions/fuzzystrmatch)
- [hstore](https://neon.com/docs/extensions/hstore)
- [intarray](https://neon.com/docs/extensions/intarray)
- [ltree](https://neon.com/docs/extensions/ltree)
- [neon](https://neon.com/docs/extensions/neon)
- [neon_utils](https://neon.com/docs/extensions/neon-utils)
- [online_advisor](https://neon.com/docs/extensions/online_advisor)
- [pgcrypto](https://neon.com/docs/extensions/pgcrypto)
- [pgvector](https://neon.com/docs/extensions/pgvector)
- [pgrag](https://neon.com/docs/extensions/pgrag)
- [pg_cron](https://neon.com/docs/extensions/pg_cron)
- [pg_graphql](https://neon.com/docs/extensions/pg_graphql)
- [pg_mooncake](https://neon.com/docs/extensions/pg_mooncake)
- [pg_partman](https://neon.com/docs/extensions/pg_partman)
- [pg_prewarm](https://neon.com/docs/extensions/pg_prewarm)
- [pg_session_jwt](https://neon.com/docs/extensions/pg_session_jwt)
- [pg_stat_statements](https://neon.com/docs/extensions/pg_stat_statements)
- [pg_repack](https://neon.com/docs/extensions/pg_repack)
- [pg_search](https://neon.com/docs/extensions/pg_search)
- [pg_tiktoken](https://neon.com/docs/extensions/pg_tiktoken)
- [pg_trgm](https://neon.com/docs/extensions/pg_trgm)
- [pg_uuidv7](https://neon.com/docs/extensions/pg_uuidv7)
- [pgrowlocks](https://neon.com/docs/extensions/pgrowlocks)
- [pgstattuple](https://neon.com/docs/extensions/pgstattuple)
- [plv8](https://neon.com/docs/extensions/plv8)
- [postgis](https://neon.com/docs/extensions/postgis)
- [postgis-related](https://neon.com/docs/extensions/postgis-related-extensions)
- [postgres_fdw](https://neon.com/docs/extensions/postgres_fdw)
- [tablefunc](https://neon.com/docs/extensions/tablefunc)
- [timescaledb](https://neon.com/docs/extensions/timescaledb)
- [unaccent](https://neon.com/docs/extensions/unaccent)
- [uuid-ossp](https://neon.com/docs/extensions/uuid-ossp)
- [wal2json](https://neon.com/docs/extensions/wal2json)
- [xml2](https://neon.com/docs/extensions/xml2)
