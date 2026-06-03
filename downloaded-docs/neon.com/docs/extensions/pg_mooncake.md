> This page location: Extensions > pg_mooncake
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup of the pg_mooncake extension for Postgres, enabling fast analytics through columnstore tables and DuckDB execution, allowing efficient data retrieval and integration with object stores.

# The pg_mooncake extension

Fast analytics in Postgres with columnstore tables and DuckDB execution

The [pg_mooncake](https://github.com/Mooncake-Labs/pg_mooncake) extension enables fast analytic workloads in Postgres by adding native columnstore tables and vectorized execution (DuckDB).

Columnstore tables improve analytical queries by storing data vertically, enabling compression and efficient column-specific retrieval with vectorized execution.

`pg_mooncake` columnstore tables are designed so that only metadata is stored in Postgres, while data is stored in an object store as Parquet files with [Iceberg](https://iceberg.apache.org/)or [Delta Lake](https://delta.io/) metadata.

Queries on `pg_mooncake` columnstore tables are executed by DuckDB.

The extension is maintained by [Mooncake Labs](https://www.mooncake.dev/).

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

You can create and use `pg_mooncake` columnstore tables like regular Postgres heap tables to run:

- Transactional `INSERT`, `SELECT`, `UPDATE`, `DELETE`, and `COPY` operations
- Joins with regular Postgres tables

In addition, you can:

- Load Parquet, CSV, and JSON files into columnstore tables
- Load Hugging Face datasets
- Run DuckDB specific aggregate functions like `approx_count_distinct`
- Read existing Iceberg and Delta Lake tables
- Write Delta Lake tables from Postgres tables

**Note:** `pg_mooncake` is an open-source extension for Postgres that can be installed on any Neon Project using the instructions below.

## Use cases for pg_mooncake

`pg_mooncake` supports several use cases, including:

1. Analytics on Postgres data
2. Time Series & Log Analytics
3. Exporting Postgres tables to your Lake or Lakehouse
4. Querying and updating existing Lakehouse tables and Parquet files directly in Postgres

This guide provides a quickstart to the `pg_mooncake` extension.

## Enable the extension

**Note:** The `pg_mooncake` extension is currently in Beta and classified as experimental in Neon. A separate, dedicated Neon project is recommended when using an extension that is still in Beta. For additional guidance, see [Experimental extensions](https://neon.com/docs/extensions/pg-extensions#experimental-extensions).

While the `pg_mooncake` extension is in Beta, you need to explicitly allow it to be used on Neon before you can install it. To do so, connect to your Neon database via an SQL client like [psql](https://neon.com/docs/connect/query-with-psql-editor) or the [Neon SQL Editor](https://neon.com/docs/get-started/query-with-neon-sql-editor) and run the `SET` command shown below.

```sql
SET neon.allow_unstable_extensions='true';
```

Install the extension:

```sql
CREATE EXTENSION pg_mooncake;
```

## Set up your object store

Run the commands outlined in the following steps on your Neon database to setup your object store.

_If you don't have an object storage bucket, you can get a free S3 express bucket [here](https://s3.pgmooncake.com/). When using the free s3 bucket, the `SELECT` and `SET` statements defined below are generated for you, which you can copy and run._

Add your object storage credentials. In this case, S3:

```sql
SELECT mooncake.create_secret('<name>', 'S3', '<key_id>',
        '<secret>', '{"REGION": "<s3-region>"}');
```

Set your default bucket:

```sql
SET mooncake.default_bucket = 's3://<bucket>';
```

**Note: R2 and GCP buckets also supported**

The `pg_mooncake` extension also supports R2 and GCP buckets. For set up instructions, refer to **pg_mooncake's** [cloud storage docs](https://pgmooncake.com/docs/cloud-storage).

In the future, you will not have to bring your own bucket to use `pg_mooncake` with Neon.

## Create a columnstore table with `USING columnstore`

Run the following SQL statement on your Neon database to create a columnstore table:

```sql
CREATE TABLE reddit_comments(
    author TEXT,
    body TEXT,
    controversiality BIGINT,
    created_utc BIGINT,
    link_id TEXT,
    score BIGINT,
    subreddit TEXT,
    subreddit_id TEXT,
    id TEXT
) using columnstore;
```

## Load data

You can find a list of data sources [here](https://pgmooncake.com/docs/load-data).
This dataset has 13 million rows and may take a few minutes to load.

```sql
INSERT INTO reddit_comments
(SELECT author, body, controversiality, created_utc, link_id, score, subreddit, subreddit_id, id
FROM mooncake.read_parquet('hf://datasets/fddemarco/pushshift-reddit-comments/data/RC_2012-01.parquet')
AS (author TEXT, body TEXT, controversiality BIGINT, created_utc BIGINT, link_id TEXT, score BIGINT, subreddit TEXT, subreddit_id TEXT, id TEXT));
```

## Query the table

Queries on columnstore tables are executed by DuckDB. For example, this aggregate query runs in ~200 milliseconds on 13 million rows:

```sql
-- Top commenters (excluding [deleted] users)
SELECT
    author,
    COUNT(*) as comment_count,
    AVG(score) as avg_score,
    SUM(score) as total_score
FROM reddit_comments
WHERE author != '[deleted]'
GROUP BY author
ORDER BY comment_count DESC
LIMIT 10;
```

## References

- [Repository](https://github.com/Mooncake-Labs/pg_mooncake)
- [Documentation](https://pgmooncake.com/docs)
- [Architecture](https://www.mooncake.dev/blog/how-we-built-pgmooncake)
- [YouTube demo](https://youtu.be/QDNsxw_3ris?feature=shared\&t=2048)

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
