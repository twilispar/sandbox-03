> This page location: Logical replication > Getting started
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup of logical replication in Neon Postgres, enabling data streaming to and from external sources, live migrations, and inter-project data replication using a publisher-subscriber model.

# Get started with logical replication

Learn how to replicate data to and from your Neon Postgres database

Neon's logical replication feature, available to all Neon users, allows you to replicate data to and from your Neon Postgres database:

- Stream data from your Neon database to external destinations, enabling Change Data Capture (CDC) and real-time analytics. External sources might include data warehouses, analytical database services, real-time stream processing systems, messaging and event-streaming platforms, and external Postgres databases, among others. See [Replicate data from Neon](https://neon.com/docs/guides/logical-replication-guide#replicate-data-from-neon).
- Perform live migrations to Neon from external sources such as AWS RDS and Google Cloud SQL — or any platform that runs Postgres. See [Replicate data to Neon](https://neon.com/docs/guides/logical-replication-guide#replicate-data-to-neon).
- Replicate data from one Neon project to another for Neon project, account, Postgres version, or region migration. See [Replicate data from one Neon project to another](https://neon.com/docs/guides/logical-replication-neon-to-neon).

![Neon logical replication subscribers image](https://neon.com/docs/guides/logical_replication_publishers_subscribers.jpg)

Logical replication in Neon works like it does on any standard Postgres installation. It uses a publisher-subscriber model to replicate data from the source database to the destination database. Neon can act as a publisher or subscriber.

Replication starts by copying a snapshot of the data from the publisher to the subscriber. Once this is done, subsequent changes are sent to the subscriber as they occur in real-time.

To learn more about Postgres logical replication, see the following topics.

## Learn about logical replication

- [Logical replication concepts](https://neon.com/docs/guides/logical-replication-concepts): Learn about Postgres logical replication concepts
- [Logical replication commands](https://neon.com/docs/guides/logical-replication-manage): Commands for managing your logical replication configuration
- [Logical replication in Neon](https://neon.com/docs/guides/logical-replication-neon): Information about logical replication specific to Neon
- [Managing schema changes](https://neon.com/docs/guides/logical-replication-schema-changes): Learn about managing schema changes in a logical replication setup

To get started, jump into one of our step-by-step logical replication guides.

## Replicate data from Neon

**Note:** To replicate data from Neon, you must first enable logical replication on your project. This setting cannot be reverted once enabled. See [Enable logical replication](https://neon.com/docs/guides/logical-replication-neon#enable-logical-replication) for Console and API instructions.

- [Airbyte](https://neon.com/docs/guides/logical-replication-airbyte): Replicate data from Neon with Airbyte
- [Bemi](https://neon.com/docs/guides/bemi): Create an automatic audit trail with Bemi
- [ClickHouse](https://neon.com/docs/guides/logical-replication-clickhouse): Replicate data from Neon to ClickHouse using ClickPipes
- [Confluent (Kafka)](https://neon.com/docs/guides/logical-replication-kafka-confluent): Replicate data from Neon with Confluent (Kafka)
- [Databricks](https://neon.com/docs/guides/logical-replication-databricks): Replicate data from Neon to Databricks
- [Decodable](https://neon.com/docs/guides/logical-replication-decodable): Replicate data from Neon with Decodable
- [Estuary Flow](https://neon.com/docs/guides/logical-replication-estuary-flow): Replicate data from Neon with Estuary Flow
- [Fivetran](https://neon.com/docs/guides/logical-replication-fivetran): Replicate data from Neon with Fivetran
- [Materialize](https://neon.com/docs/guides/logical-replication-materialize): Replicate data from Neon to Materialize
- [Neon to Neon](https://neon.com/docs/guides/logical-replication-neon-to-neon): Replicate data from Neon to Neon
- [Neon to PostgreSQL](https://neon.com/docs/guides/logical-replication-postgres): Replicate data from Neon to PostgreSQL
- [Prisma Pulse](https://neon.com/docs/guides/logical-replication-prisma-pulse): Stream database changes in real-time with Prisma Pulse
- [Sequin](https://neon.com/docs/guides/sequin): Stream data from platforms like Stripe, Linear, and GitHub to Neon
- [Snowflake](https://neon.com/docs/guides/logical-replication-airbyte-snowflake): Replicate data from Neon to Snowflake with Airbyte
- [Inngest](https://neon.com/docs/guides/logical-replication-inngest): Replicate data from Neon to Inngest

## Replicate data to Neon

- [AlloyDB](https://neon.com/docs/guides/logical-replication-alloydb): Replicate data from AlloyDB to Neon
- [Azure PostgreSQL](https://neon.com/docs/import/migrate-from-azure-postgres): Replicate data from Azure PostgreSQL to Neon
- [Cloud SQL](https://neon.com/docs/guides/logical-replication-cloud-sql): Replicate data from Cloud SQL to Neon
- [Neon to Neon](https://neon.com/docs/guides/logical-replication-neon-to-neon): Replicate data from Neon to Neon
- [PostgreSQL to Neon](https://neon.com/docs/guides/logical-replication-postgres-to-neon): Replicate data from PostgreSQL to Neon
- [RDS](https://neon.com/docs/guides/logical-replication-rds-to-neon): Replicate data from AWS RDS PostgreSQL to Neon
- [Supabase](https://neon.com/docs/guides/logical-replication-supabase-to-neon): Replicate data from Supabase to Neon

---

## Related docs (Logical replication)

- [Concepts](https://neon.com/docs/guides/logical-replication-concepts)
- [In Neon](https://neon.com/docs/guides/logical-replication-neon)
- [Commands](https://neon.com/docs/guides/logical-replication-manage)
- [Schema changes](https://neon.com/docs/guides/logical-replication-schema-changes)
- [Tips](https://neon.com/docs/guides/logical-replication-tips)
