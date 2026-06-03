> This page location: Extensions > anon
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup of the `anon` extension for data masking and anonymization in Postgres databases, enabling protection of sensitive information and compliance with regulations like GDPR.

# The anon extension

Protecting sensitive data in Postgres databases

The `anon` extension ([PostgreSQL Anonymizer](https://postgresql-anonymizer.readthedocs.io)) provides data masking and anonymization capabilities to protect sensitive data in Postgres databases. It helps protect personally identifiable information (PII) and other sensitive data, facilitating compliance with regulations such as [GDPR](https://gdpr-info.eu/).

**Note:** This extension comes from the [PostgreSQL Anonymizer](https://postgresql-anonymizer.readthedocs.io) open source project (`postgresql_anonymizer`). This is distinct from other tools such as `pg_anon`. The extension is installed using `CREATE EXTENSION anon`.

**Tip: Looking for a practical guide?**

For complete step-by-step workflows on anonymizing data in Neon branches, including manual procedures and GitHub Actions automation, see [data anonymization](https://neon.com/docs/workflows/data-anonymization).

## Enable the extension

**Note:** This extension is currently [experimental](https://neon.com/docs/extensions/pg-extensions#experimental-extensions) and may change in future releases.

When using the Neon Console or API for anonymization workflows, the extension is enabled automatically. It can also be enabled manually using SQL commands.

### Enable via SQL

When working with SQL-based workflows (such as using `psql` or other SQL clients), enable the `anon` extension in your Neon database by following these steps:

1. Connect to your Neon database using either the [Neon SQL Editor](https://neon.com/docs/get-started/query-with-neon-sql-editor) or an SQL client like [psql](https://neon.com/docs/connect/query-with-psql-editor)

2. Enable experimental extensions:

   ```sql
   SET neon.allow_unstable_extensions='true';
   ```

3. Install the extension:

   ```sql
   CREATE EXTENSION IF NOT EXISTS anon;
   ```

**Tip:** When using the Neon Console or API to create branches, the extension is enabled automatically. See the [data anonymization workflow guide](https://neon.com/docs/workflows/data-anonymization) for details.

## Masking rules

Masking rules define which data to mask and how to mask it using SQL syntax. These rules are applied using `SECURITY LABEL` SQL commands and stored within the database schema to implement the privacy by design principle.

## Masking functions

PostgreSQL Anonymizer provides [built-in functions](https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/) for different anonymization requirements, including but not limited to:

| Function Type      | Description                                           | Example                                                                                         |
| ------------------ | ----------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Faking             | Generate realistic data                               | `anon.fake_first_name()` and `anon.lorem_ipsum()`                                               |
| Pseudonymization   | Create consistent and reversible fake data            | `anon.pseudo_email(seed)`                                                                       |
| Randomization      | Generate random values                                | `anon.random_int_between(10, 100)` and `anon.random_in_enum(enum_column)`                       |
| Partial scrambling | Hide portions of strings                              | `anon.partial(ip_address, 8, ''XXX.XXX'', 0)` would change `192.168.1.100` to `192.168.XXX.XXX` |
| Nullification      | Replace with static values or `NULL`                  | `MASKED WITH VALUE 'CONFIDENTIAL'`                                                              |
| Noise addition     | Alter numerical values while maintaining distribution | `anon.noise(salary, 0.1)` adds `+/- 10%` noise to the `salary` column                           |
| Generalization     | Replace specific values with broader categories       | `anon.generalize_int4range(age, 10)` would change `54` to `[50,60)`                             |

## Static masking

Static masking permanently modifies the original data in your tables. This approach is useful for creating anonymized copies of data when:

- Migrating production data to development branches
- Creating sanitized datasets for testing
- Archiving data with sensitive information removed
- Distributing data to third parties

### Branch operations and static masking

When using Neon's branch features with static masking:

- Creating a child branch copies all data as-is from the parent
- Resetting a branch from the parent replaces all branch data with the parent's current state
- In both cases, any previous anonymization is lost and must be reapplied

## Practical examples

For complete implementation examples showing how to apply these masking functions in real workflows, see the [data anonymization guide](https://neon.com/docs/workflows/data-anonymization), which covers:

- Creating and anonymizing development branches
- Applying different masking strategies to protect sensitive data
- Automating anonymization with GitHub Actions
- Best practices and safety tips

## Limitations

- Neon currently only supports static masking with this extension
- With static masking, branch reset operations restore original data, requiring anonymization to be run again
- Additional `pg_catalog` functions cannot be declared as `TRUSTED` in Neon's implementation

## Conclusion

This extension provides a toolkit for protecting sensitive data in Postgres databases.
By defining appropriate masking rules, you can create anonymized datasets that maintain usability while protecting individual privacy.

## Reference

- [Data anonymization workflow guide](https://neon.com/docs/workflows/data-anonymization) - Practical guide for anonymizing data in Neon branches
- [PostgreSQL Anonymizer Repository](https://gitlab.com/dalibo/postgresql_anonymizer)
- [Official Documentation](https://postgresql-anonymizer.readthedocs.io/en/latest/)
- [Masking Functions Reference](https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/)

---

## Related docs (Extensions)

- [Extension explorer](https://neon.com/docs/extensions/extension-explorer)
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
- [wal2json](https://neon.com/docs/extensions/wal2json)
- [xml2](https://neon.com/docs/extensions/xml2)
