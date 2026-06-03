> This page location: Migrate to Neon > Overview
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the selection of migration methods for transferring data to Neon Postgres from various database sources, considering factors like database size, downtime tolerance, and technical skill requirements.

# Neon data migration guides

Learn how to migrate data to Neon Postgres from different database providers and sources

This guide helps you choose the best migration method based on your database size, downtime tolerance, source database type, and technical requirements.

## Migration methods

| Method                                                                          | Best For                               | Database Size | Downtime                | Technical Skill | Key Benefit                   |
| ------------------------------------------------------------------------------- | -------------------------------------- | ------------- | ----------------------- | --------------- | ----------------------------- |
| [Import Data Assistant](https://neon.com/docs/import/import-data-assistant)     | Quick Postgres migrations              | Under 10GB    | Minimal (minutes–hours) | Low             | Easiest - fully automated     |
| [pg\_dump/restore](https://neon.com/docs/import/migrate-from-postgres)          | Standard Postgres migrations           | Any size      | Required                | Medium          | Reliable and well-tested      |
| [pgcopydb](https://neon.com/docs/import/pgcopydb)                               | Large Postgres databases               | 10GB+         | Required                | Medium          | Parallel processing - fast    |
| [Logical Replication](https://neon.com/docs/guides/logical-replication-guide)   | Production Postgres workloads          | Any size      | Near-zero               | High            | Minimal downtime              |
| [pgloader](https://neon.com/docs/import/migrate-intro#provider-specific-guides) | Non-Postgres sources                   | Any size      | Required                | Medium          | Handles MySQL, MSSQL, SQLite  |
| [AWS DMS](https://neon.com/docs/import/migrate-aws-dms)                         | Multi-source or custom transformations | Any size      | Minimal (minutes–hours) | High            | Advanced transformation rules |

**Tip: Quick guidance**

If you can't afford downtime, use [Logical Replication](https://neon.com/docs/guides/logical-replication-guide). For Postgres databases under 10GB with some downtime flexibility, [Import Data Assistant](https://neon.com/docs/import/import-data-assistant) is the easiest option. For larger Postgres databases where downtime is acceptable, choose between [pg_dump/restore](https://neon.com/docs/import/migrate-from-postgres) (simplest) or [pgcopydb](https://neon.com/docs/import/pgcopydb) (fastest).

## Region migration

If you need your Neon **database** in a different **region**, or a **Postgres-compatible export** from Neon, start with **[Region migration](https://neon.com/docs/import/region-migration)** for paths and tradeoffs. For **Neon-to-Neon** moves, use **[Migrate to another Neon region](https://neon.com/docs/import/migrate-neon-to-another-region)**. A project stays in one region; you create a **new** Neon project in the target region and migrate your **data**, or export. For a **piped** `pg_dump | pg_restore` between Neon projects, see **[Migrate data from another Neon project](https://neon.com/docs/import/migrate-from-neon)**.

## Provider-specific guides

For step-by-step instructions tailored to specific databases or providers, see [MySQL](https://neon.com/docs/import/migrate-mysql), [MSSQL](https://neon.com/docs/import/migrate-mssql), [SQLite](https://neon.com/docs/import/migrate-sqlite), [Heroku](https://neon.com/docs/import/migrate-from-heroku), [Supabase](https://neon.com/docs/import/migrate-from-supabase), [PlanetScale](https://neon.com/docs/import/migrate-from-planetscale), [Turso](https://neon.com/docs/import/migrate-from-turso), [Render](https://neon.com/docs/import/migrate-from-render), [Azure](https://neon.com/docs/import/migrate-from-azure-postgres), [Digital Ocean](https://neon.com/docs/import/migrate-from-digital-ocean), [Railway](https://neon.com/docs/import/migrate-from-railway), [Firebase](https://neon.com/docs/import/migrate-from-firebase), or [another Neon project](https://neon.com/docs/import/migrate-from-neon).

## Logical replication guides

For near-zero downtime Postgres database migrations using logical replication, see guides for [AWS RDS](https://neon.com/docs/guides/logical-replication-rds-to-neon), [Google Cloud SQL](https://neon.com/docs/guides/logical-replication-cloud-sql), [AlloyDB](https://neon.com/docs/guides/logical-replication-alloydb), [Azure](https://neon.com/docs/import/migrate-from-azure-postgres), [Supabase](https://neon.com/docs/guides/logical-replication-supabase-to-neon), [PostgreSQL](https://neon.com/docs/guides/logical-replication-postgres-to-neon), or [Neon to Neon](https://neon.com/docs/guides/logical-replication-neon-to-neon).

## Other imports

- [Import data from CSV](https://neon.com/docs/import/import-from-csv): Import data from CSV files using psql
- [Import sample data](https://neon.com/docs/import/import-sample-data): Try Neon with sample datasets
- [Migrate schema only](https://neon.com/docs/import/migrate-schema-only): Migrate just the schema without data

---

## Related docs (Migrate to Neon)

- [CSV](https://neon.com/docs/import/import-from-csv)
- [Sample data](https://neon.com/docs/import/import-sample-data)
