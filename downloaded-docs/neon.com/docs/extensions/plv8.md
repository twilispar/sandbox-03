> This page location: Extensions > plv8
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: plv8 is deprecated on Neon due to security concerns. New installations are blocked; use plpgsql or application-side logic instead.

# The plv8 extension (deprecated)

JavaScript procedural language for Postgres — no longer available on Neon

**Warning: The plv8 extension is deprecated on Neon.**

**plv8** is deprecated because of **security risks** tied to embedding the V8 JavaScript engine inside Postgres. Neon blocks **`CREATE EXTENSION plv8`** with an explicit deprecation message (same mechanism as other deprecated extensions).

**If you use plv8 today:** move business logic out of JavaScript-in-SQL—for example rewrite functions in **[plpgsql](https://www.postgresql.org/docs/current/plpgsql.html)** or run JavaScript in your **application** or **worker** layer—and then drop **`plv8`** from databases that still have it. See also [Supported Postgres extensions](https://neon.com/docs/extensions/pg-extensions).

**Alternatives:**

- **SQL and built-in languages:** [plpgsql](https://www.postgresql.org/docs/current/plpgsql.html) (Postgres default procedural language); [plpgsql_check](https://pgxn.org/dist/plpgsql_check/) for optional validation in CI
- **Calling external services:** use **`http`** patterns from app code, queues, or **`pg_net`** / HTTP from Postgres where appropriate for your stack

Upstream project (reference only): [plv8 on GitHub](https://github.com/plv8/plv8).

Neon previously shipped **plv8** so you could write Postgres functions in JavaScript via the V8 engine. That capability is **no longer offered** on new installs.

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
- [postgis](https://neon.com/docs/extensions/postgis)
- [postgis-related](https://neon.com/docs/extensions/postgis-related-extensions)
- [postgres_fdw](https://neon.com/docs/extensions/postgres_fdw)
- [tablefunc](https://neon.com/docs/extensions/tablefunc)
- [timescaledb](https://neon.com/docs/extensions/timescaledb)
- [unaccent](https://neon.com/docs/extensions/unaccent)
- [uuid-ossp](https://neon.com/docs/extensions/uuid-ossp)
- [wal2json](https://neon.com/docs/extensions/wal2json)
- [xml2](https://neon.com/docs/extensions/xml2)
