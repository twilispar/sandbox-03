> This page location: Extensions > wal2json
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup and configuration of the `wal2json` plugin for Postgres in Neon, enabling the conversion of Write-Ahead Log changes into JSON format for easier processing in applications like data replication and real-time analytics.

# The wal2json plugin

Convert Postgres Write-Ahead Log (WAL) changes to JSON format

The `wal2json` plugin is a logical replication decoding output plugin for Postgres. It lets you convert the Write-Ahead Log (WAL) changes into JSON format, making it easier to consume and process database changes in various applications, such as data replication, auditing, event-driven services, and real-time analytics.

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

This guide describes the `wal2json` plugin — how to enable it in Neon, configure its output, and use it to capture and process database changes in JSON format. WAL decoding is crucial for building robust data pipelines, implementing Change Data Capture (CDC) systems, and maintaining data consistency across distributed systems.

**Note:** The `wal2json` plugin is included in your Neon project and doesn't require a separate installation.

**Version availability:**

The `wal2json` plugin is available in all Postgres versions supported by Neon. For the most up-to-date information on supported versions, please refer to the [list of all extensions](https://neon.com/docs/extensions/pg-extensions) available in Neon.

## Enable logical replication

Before using the `wal2json` plugin, you need to enable logical replication for your Neon project. Navigate to the **Settings** page in your Neon Project Dashboard, and select **Beta** from the list of options. Click **Enable** to enable logical replication.

**Note:** Once enabled for a project, logical replication cannot be reverted. This action triggers a restart of all active compute endpoints in your Neon project. Any active connections will be dropped and have to reconnect.

To verify that logical replication is enabled, navigate to the `SQL Editor` and verify the output of the following query:

```sql
SHOW wal_level;

 wal_level
-----------
 logical
(1 row)
```

For information about using the Neon SQL Editor, see [Query with Neon's SQL Editor](https://neon.com/docs/get-started/query-with-neon-sql-editor). For information about using the `psql` client with Neon, see [Connect with psql](https://neon.com/docs/connect/query-with-psql-editor).

## Create a replication slot

To start using `wal2json`, you first need to create a replication slot that explicitly specifies `wal2json` as the decoder plugin. You can do this by running the following query:

```sql
SELECT 'start' FROM pg_create_logical_replication_slot('test_slot', 'wal2json');
```

This creates a replication slot named `test_slot` using the `wal2json` plugin. Now, we can query this slot to listen for changes to any tables in the database.

## Example - use `wal2json` to capture changes to a table

Suppose we have a table named `inventory` that stores information about products for a retail store. We want to capture changes to this table in real-time and process them using `wal2json`.

Run the following query to create the `inventory` table, and insert some sample data:

```sql
CREATE TABLE inventory (
    id SERIAL PRIMARY KEY,
    product_name VARCHAR(100),
    quantity INTEGER,
    last_updated TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO inventory (product_name, quantity) VALUES
    ('Widget A', 100),
    ('Gadget B', 50),
    ('Gizmo C', 75);
```

With logical decoding enabled, Postgres streams changes to the `inventory` table to the `test_slot` replication slot. Run the following query to observe the messages that have been published to it:

```sql
SELECT * FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', 'on');
```

This query returns the changes in JSON format. Each change will be represented as a separate JSON object.

```plaintext
    lsn    | xid  |                                                          data
-----------+------+-------------------------------------------------------------------------------------------------------------------------
 0/24E7950 | 2055 | {                                                                                                                      +
           |      |         "change": [                                                                                                    +
           |      |         ]                                                                                                              +
           |      | }
 0/24E7D60 | 2056 | {                                                                                                                      +
           |      |         "change": [                                                                                                    +
           |      |                 {                                                                                                      +
           |      |                         "kind": "insert",                                                                              +
           |      |                         "schema": "public",                                                                            +
           |      |                         "table": "inventory",                                                                          +
           |      |                         "columnnames": ["id", "product_name", "quantity", "last_updated"],                             +
           |      |                         "columntypes": ["integer", "character varying(100)", "integer", "timestamp without time zone"],+
           |      |                         "columnvalues": [1, "Widget A", 100, "2024-07-30 09:53:26.078749"]                             +
           |      |                 }                                                                                                      +
           |      |                 ,{                                                                                                     +
           |      |                         "kind": "insert",                                                                              +
           |      |                         "schema": "public",                                                                            +
           |      |                         "table": "inventory",                                                                          +
           |      |                         "columnnames": ["id", "product_name", "quantity", "last_updated"],                             +
           |      |                         "columntypes": ["integer", "character varying(100)", "integer", "timestamp without time zone"],+
           |      |                         "columnvalues": [2, "Gadget B", 50, "2024-07-30 09:53:26.078749"]                              +
           |      |                 }                                                                                                      +
           |      |                 ,{                                                                                                     +
           |      |                         "kind": "insert",                                                                              +
           |      |                         "schema": "public",                                                                            +
           |      |                         "table": "inventory",                                                                          +
           |      |                         "columnnames": ["id", "product_name", "quantity", "last_updated"],                             +
           |      |                         "columntypes": ["integer", "character varying(100)", "integer", "timestamp without time zone"],+
           |      |                         "columnvalues": [3, "Gizmo C", 75, "2024-07-30 09:53:26.078749"]                               +
           |      |                 }                                                                                                      +
           |      |         ]                                                                                                              +
           |      | }
(2 rows)
```

There are two rows in the query output above. The first row corresponds to the `CREATE TABLE` statement that we ran earlier. Logical decoding only captures information about DML (data manipulation) events — `INSERT`, `UPDATE`, and `DELETE` statements, hence this row is empty. The second row corresponds to the `INSERT` statement that added rows to the `inventory` table.

Next, we update an existing row in the `inventory` table:

```sql
UPDATE inventory SET quantity = quantity + 100 WHERE product_name = 'Widget A';
```

We can now query the `test_slot` replication slot again to see the new information published as a result of the update:

```sql
SELECT * FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', 'on');
```

This query returns a single row in JSON format, corresponding to the row updated.

```plaintext
    lsn    | xid  |                                                          data
-----------+------+-------------------------------------------------------------------------------------------------------------------------
 0/24EC940 | 2057 | {                                                                                                                      +
           |      |         "change": [                                                                                                    +
           |      |                 {                                                                                                      +
           |      |                         "kind": "update",                                                                              +
           |      |                         "schema": "public",                                                                            +
           |      |                         "table": "inventory",                                                                          +
           |      |                         "columnnames": ["id", "product_name", "quantity", "last_updated"],                             +
           |      |                         "columntypes": ["integer", "character varying(100)", "integer", "timestamp without time zone"],+
           |      |                         "columnvalues": [1, "Widget A", 200, "2024-07-30 09:53:26.078749"],                            +
           |      |                         "oldkeys": {                                                                                   +
           |      |                                 "keynames": ["id"],                                                                    +
           |      |                                 "keytypes": ["integer"],                                                               +
           |      |                                 "keyvalues": [1]                                                                       +
           |      |                         }                                                                                              +
           |      |                 }                                                                                                      +
           |      |         ]                                                                                                              +
           |      | }
(1 row)
```

## Format versions: 1 vs 2

The `wal2json` plugin supports two different output format versions.

The default format version is 1, which produces a JSON object per transaction. All new and old tuples are available within this single JSON object. This format is useful when you need to process entire transactions as atomic units.

Format version 2 produces a JSON object per tuple (row), with optional JSON objects for the beginning and end of each transaction. This format is more granular and can be useful when you need to process changes on a row-by-row basis. Both formats support various options to include additional properties such as transaction timestamps, schema-qualified names, data types, and transaction IDs.

To use format version 2, you need to specify it explicitly:

```sql
SELECT * FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'format-version', '2');
```

To illustrate, we add a couple more product entries to the `inventory` table:

```sql
INSERT INTO inventory (product_name, quantity) VALUES
    ('Widget D', 200),
    ('Gizmo E', 75);
```

Now, we can query the `test_slot` replication slot again to see the new information published as a result of the update:

```sql
SELECT * FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', 'on', 'format-version', '2');
```

The output of this query appears as follows. You can see that there is a separate JSON object for each row inserted.

```plaintext
    lsn    | xid  |                                                                                                                                                                  data
-----------+------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 0/24F18D8 | 3078 | {"action":"B"}
 0/24F1940 | 3078 | {"action":"I","schema":"public","table":"inventory","columns":[{"name":"id","type":"integer","value":8},{"name":"product_name","type":"character varying(100)","value":"Widget D"},{"name":"quantity","type":"integer","value":200},{"name":"last_updated","type":"timestamp without time zone","value":"2024-07-30 10:27:45.428407"}]}
 0/24F1A48 | 3078 | {"action":"I","schema":"public","table":"inventory","columns":[{"name":"id","type":"integer","value":9},{"name":"product_name","type":"character varying(100)","value":"Gizmo E"},{"name":"quantity","type":"integer","value":75},{"name":"last_updated","type":"timestamp without time zone","value":"2024-07-30 10:27:45.428407"}]}
 0/24F1B10 | 3078 | {"action":"C"}
(4 rows)
```

## Use `wal2json` with tables without a primary key

`REPLICA IDENTITY` is a table property that determines what information is written to the WAL when a row is updated or deleted. This property is crucial for `wal2json` when working with tables that don't have a primary key.

`REPLICA IDENTITY` has four possible settings:

1. `DEFAULT`: Only primary key columns are logged for `UPDATE` and `DELETE` operations.
2. `USING INDEX`: A specified index's columns are logged for `UPDATE` and `DELETE` operations.
3. `FULL`: All columns are logged for `UPDATE` and `DELETE` operations.
4. `NOTHING`: No information is logged for `UPDATE` and `DELETE` operations.

Tables use the `DEFAULT` setting by default. For tables without a primary key, this means no information is logged for updates and deletes. Let's create a table without a primary key and see how `wal2json` behaves:

```sql
CREATE TABLE products_no_pk (
    product_name VARCHAR(100),
    quantity INTEGER,
    price DECIMAL(10, 2)
);

INSERT INTO products_no_pk (product_name, quantity, price) VALUES ('Widget', 100, 19.99);
UPDATE products_no_pk SET quantity = 90 WHERE product_name = 'Widget';
```

The `wal2json` output for this update operation will not contain any information about the updated row due to the lack of a primary key and the `DEFAULT REPLICA IDENTITY` setting.

```plaintext
WARNING:  table "products_no_pk" without primary key or replica identity is nothing
    lsn    | xid  |        data
-----------+------+---------------------
 0/256D6C8 | 6151 | {                  +
           |      |         "change": [+
           |      |         ]          +
           |      | }
(1 row)
```

To capture changes for tables without a primary key, we can change the `REPLICA IDENTITY` to `FULL`:

```sql
ALTER TABLE products_no_pk REPLICA IDENTITY FULL;
UPDATE products_no_pk SET price = 21.99 WHERE product_name = 'Widget';
```

Now, the `wal2json` output will include both the old and new values for all columns, which can be used to identify the changed row. To verify, we can query the `test_slot` replication slot again:

```sql
SELECT * FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', 'on');
```

The output of this query appears as follows:

```plaintext
    lsn    | xid  |                                                data
-----------+------+-----------------------------------------------------------------------------------------------------
 0/256E228 | 6152 | {                                                                                                  +
           |      |         "change": [                                                                                +
           |      |         ]                                                                                          +
           |      | }
 0/256E310 | 6153 | {                                                                                                  +
           |      |         "change": [                                                                                +
           |      |                 {                                                                                  +
           |      |                         "kind": "update",                                                          +
           |      |                         "schema": "public",                                                        +
           |      |                         "table": "products_no_pk",                                                 +
           |      |                         "columnnames": ["product_name", "quantity", "price"],                      +
           |      |                         "columntypes": ["character varying(100)", "integer", "numeric(10,2)"],     +
           |      |                         "columnvalues": ["Widget", 90, 21.99],                                     +
           |      |                         "oldkeys": {                                                               +
           |      |                                 "keynames": ["product_name", "quantity", "price"],                 +
           |      |                                 "keytypes": ["character varying(100)", "integer", "numeric(10,2)"],+
           |      |                                 "keyvalues": ["Widget", 90, 19.99]                                 +
           |      |                         }                                                                          +
           |      |                 }                                                                                  +
           |      |         ]                                                                                          +
           |      | }
(2 rows)
```

## Performance considerations

When working with `wal2json`, keep the following performance considerations in mind:

1. **Replication slot management**: Unused replication slots can prevent WAL segments from being removed, potentially causing disk space issues. Regularly monitor and clean up unused slots.
2. **Batch processing**: Instead of processing each change individually, consider batching changes for more efficient processing.
3. **Resource usage**: Be mindful of network bandwidth usage, especially when dealing with high-volume changes or when replicating over a wide area network. Additionally, decoding WAL to JSON can be CPU-intensive. Monitor your system's CPU usage and consider scaling your resources if needed.

## Conclusion

The `wal2json` plugin is a powerful tool for capturing and processing database changes in JSON format. We've seen how to enable it, configure its output, and use it in various scenarios. Whether you're implementing a data replication system, building an audit trail, or creating an event-driven architecture, `wal2json` provides a flexible and efficient way to work with the Postgres Write-Ahead Log (WAL).

## Resources

- [wal2json GitHub Repository](https://github.com/eulerto/wal2json)
- [PostgreSQL Logical Decoding](https://www.postgresql.org/docs/current/logicaldecoding.html)
- [Manage logical replication in Neon - Decoder plugins](https://neon.com/docs/guides/logical-replication-manage#decoder-plugins)

---

## Related docs (Extensions)

- [Extension explorer](https://neon.com/docs/extensions/extension-explorer)
- [anon](https://neon.com/docs/extensions/postgresql-anonymizer)
- [btree_gin](https://neon.com/docs/extensions/btree_gin)
- [btree_gist](https://neon.com/docs/extensions/btree_gist)
- [citext](https://neon.com/docs/extensions/citext)
- [cube](https://neon.com/docs/extensions/cube)
- [dblink](https://neon.com/docs/extensions/dblink)
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
- [xml2](https://neon.com/docs/extensions/xml2)
