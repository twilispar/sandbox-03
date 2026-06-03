> This page location: Neon platform > Monitoring & observability > Active queries
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the monitoring of active queries in a Neon project through the Neon Console, detailing how to access and refresh the list of currently running queries using the `pg_stat_activity` system view.

# Monitor active queries

View and analyze running queries in your database

You can monitor active queries for your Neon project from the **Monitoring** page in the Neon Console.

1. In the Neon Console, select a project.
2. Go to **Monitoring**.
3. Select the **Active queries** tab.

The **Active queries** view displays up to 100 currently running queries for the selected **Branch**, **Compute**, and **Database**. Use the **Refresh** button to update the list with the latest active queries.

![Neon active queries tab](https://neon.com/docs/introduction/active_queries.png)

The **Active queries** view is powered by the `pg_stat_activity` Postgres system view, which is available in Neon by default. To run custom queries against the data collected by `pg_stat_activity`, you can use the [Neon SQL Editor](https://neon.com/docs/get-started/query-with-neon-sql-editor) or any SQL client, such as [psql](https://neon.com/docs/connect/query-with-psql-editor).

For details on `pg_stat_activity`, see [pg_stat_activity](https://www.postgresql.org/docs/current/monitoring-stats.html#MONITORING-PG-STAT-ACTIVITY-VIEW) in the PostgreSQL documentation.

**Note: active queries retention**

In Neon, the `pg_stat_activity` system view only holds data on currently running queries. Once a query completes, it no longer appears in the **Active queries** view. If your Neon compute scales down to zero due to inactivity, there will be no active queries until a new connection is established and a query is run.

---

## Related docs (Monitoring & observability)

- [Overview](https://neon.com/docs/introduction/monitoring)
- [Monitoring dashboard](https://neon.com/docs/introduction/monitoring-page)
- [System operations](https://neon.com/docs/manage/operations)
- [Query performance](https://neon.com/docs/introduction/monitor-query-performance)
