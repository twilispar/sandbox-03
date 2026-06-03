> This page location: Extensions > online_advisor
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup of the `online_advisor` extension, which provides recommendations for indexes, extended statistics, and prepared statements based on query workload analysis in PostgreSQL 17.

# The online_advisor extension

Get index, statistics, and prepared statement recommendations based on your query workload

The `online_advisor` extension recommends **indexes**, **extended statistics**, and **prepared statements** based on your actual query workload. It uses the same executor hook mechanism as [`auto_explain`](https://www.postgresql.org/docs/current/auto-explain.html) to collect and analyze execution data.

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

## What it does

- Suggests **indexes** when queries filter many rows
- Suggests **extended statistics** when the planner's row estimates are far off from actuals
- Identifies queries that could benefit from **prepared statements** when planning time is high

**Note:** `online_advisor` only makes recommendations. It does not create indexes or statistics for you.

## Requirements

- Supported on Postgres **17**
- Create the extension in every database you want to inspect
- Activate it by calling any provided function (for example, `get_executor_stats()`)

### Version availability

Please refer to the [Supported Postgres extensions](https://neon.com/docs/extensions/pg-extensions) page for the latest supported version of `online_advisor` on Neon.

## Enable the online_advisor extension

You can create the extension in each target database using `CREATE EXTENSION`:

```sql
CREATE EXTENSION online_advisor;
```

For information about using the Neon SQL Editor, see [Query with Neon's SQL Editor](https://neon.com/docs/get-started/query-with-neon-sql-editor). For information about using the `psql` client with Neon, see [Connect with psql](https://neon.com/docs/connect/query-with-psql-editor).

## Start collecting recommendations

1. Activate the extension by calling any function:

   ```sql
   SELECT get_executor_stats();
   ```

2. Run your workload to collect data.

3. View recommendations:

   ```sql
   -- Proposed indexes
   SELECT * FROM proposed_indexes ORDER BY elapsed_sec DESC;

   -- Proposed extended statistics
   SELECT * FROM proposed_statistics ORDER BY elapsed_sec DESC;
   ```

## Apply accepted recommendations

Run the `create_index` or `create_statistics` statement from the views, then analyze the table:

```sql
CREATE INDEX CONCURRENTLY IF NOT EXISTS idx_orders_customer_date ON orders(customer_id, order_date);
VACUUM (ANALYZE) orders;
```

## Configure thresholds

You can tune `online_advisor` with these settings:

| Setting                                  | Default | Description                                                                              |
| ---------------------------------------- | ------- | ---------------------------------------------------------------------------------------- |
| `online_advisor.filtered_threshold`      | `1000`  | Minimum filtered rows in a node to suggest an index.                                     |
| `online_advisor.misestimation_threshold` | `10`    | Minimum actual/estimated row ratio to flag misestimation.                                |
| `online_advisor.min_rows`                | `1000`  | Minimum returned rows before misestimation is considered.                                |
| `online_advisor.max_index_proposals`     | `1000`  | Max tracked clauses for index proposals (system-level, read-only on Neon).               |
| `online_advisor.max_stat_proposals`      | `1000`  | Max tracked clauses for extended statistics proposals (system-level, read-only on Neon). |
| `online_advisor.do_instrumentation`      | `on`    | Toggle data collection.                                                                  |
| `online_advisor.log_duration`            | `off`   | Log planning/execution time for each query.                                              |
| `online_advisor.prepare_threshold`       | `1.0`   | Planning/execution time ratio above which to suggest prepared statements.                |

**Note:** On Neon, you can only modify session-level settings using `SET`. System-level settings like `online_advisor.max_index_proposals` and `online_advisor.max_stat_proposals` use default values and cannot be changed. If you need different system-level settings, reach out to Neon Support.

Change a setting for the current session:

```sql
SET online_advisor.filtered_threshold = 2000;
```

## Check planning and execution stats

Use `get_executor_stats()` to see planning and execution times and whether prepared statements might help:

```sql
SELECT * FROM get_executor_stats(false); -- false = do not reset counters
```

Look at `avg_planning_overhead`. Values greater than `1` suggest that some queries would benefit from prepared statements.

## Combine or separate index proposals

By default, `online_advisor` tries to combine related predicates into a single compound index. To view separate recommendations for each predicate:

```sql
SELECT * FROM propose_indexes(false);
```

## Limitations

- Does not check operator ordering for compound indexes
- Does not suggest indexes for joins or `ORDER BY` clauses
- Does not estimate the benefit of adding an index; pair with [HypoPG](https://github.com/HypoPG/hypopg#) if you want to simulate usage
- Recommendations are per database

## Remove the extension

```sql
DROP EXTENSION IF EXISTS online_advisor;
```

If you're not using it anywhere, remove it from `shared_preload_libraries` and restart.

## Example workflow

```sql
-- Activate and run workload
SELECT get_executor_stats();

-- View index proposals
SELECT create_index, n_filtered, n_called, elapsed_sec
FROM proposed_indexes
ORDER BY elapsed_sec DESC
LIMIT 10;

-- View extended statistics proposals
SELECT create_statistics, misestimation, n_called, elapsed_sec
FROM proposed_statistics
ORDER BY misestimation DESC
LIMIT 10;

-- Apply a recommendation
CREATE INDEX CONCURRENTLY IF NOT EXISTS idx_orders_customer_date ON orders(customer_id, order_date);
VACUUM (ANALYZE) orders;

-- Check planning/execution times
SELECT * FROM get_executor_stats(true); -- reset after reading
```

## Resources

- [online_advisor GitHub repository](https://github.com/knizhnik/online_advisor)
- [PostgreSQL auto_explain documentation](https://www.postgresql.org/docs/current/auto-explain.html)

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
