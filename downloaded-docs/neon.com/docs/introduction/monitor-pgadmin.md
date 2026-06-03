> This page location: Neon platform > Monitoring & observability > Third-party monitoring > pgAdmin
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup and monitoring of a Neon Postgres database using pgAdmin, detailing installation, connection procedures, and performance metrics tracking.

# Monitor Neon with pgAdmin

Monitor your Neon Postgres database with pgAdmin

pgAdmin is a database management tool for Postgres that supports various database tasks, including monitoring performance metrics.

![PgAdmin monitoring dashboard](https://neon.com/docs/introduction/pgadmin_monitor.png)

With pgAdmin, you can monitor real-time activity for a variety of metrics including:

- Active sessions (Total, Active, and Idle)
- Transactions per second (Transactions, Commits, Rollbacks)
- Tuples in (Inserts, Updates, Deletes)
- Tuples out (Fetched, Returned)
- Block I/O for shared buffers (see [Cache your data](https://neon.com/docs/postgresql/query-performance#cache-your-data) for information about Neon's Local File Cache)
- Database activity (Sessions, Locks, Prepared Transactions)

**Note: Notes**

Neon currently does not support the `system_stats` extension required to use the **System Statistics** tab in pgAdmin. It's also important to note that pgAdmin, while active, polls your database for statistics, which does not allow your compute to suspend as it normally would when there is no other database activity.

## How to install pgAdmin

Pre-compiled and configured installation packages for pgAdmin 4 are available for different desktop environments. For installation instructions, refer to the [pgAdmin deployment documentation](https://www.pgadmin.org/docs/pgadmin4/latest/deployment.html). Downloads can be found on the [PgAdmin Downloads](https://www.pgadmin.org/download/) page.

## How to connect to your database from pgAdmin

Find the connection string for your database by clicking the **Connect** button on your **Project Dashboard**.

![Connection details modal](https://neon.com/docs/connect/connection_details.png)

Enter your connection details as shown [here](https://neon.com/docs/connect/connect-postgres-gui#connect-to-the-database).

Neon uses the default Postgres port: `5432`

---

## Related docs (Third-party monitoring)

- [Datadog](https://neon.com/docs/guides/datadog)
- [Grafana Cloud](https://neon.com/docs/guides/grafana-cloud)
- [OpenTelemetry](https://neon.com/docs/guides/opentelemetry)
- [PgHero](https://neon.com/docs/introduction/monitor-pghero)
- [Metrics and logs reference](https://neon.com/docs/reference/metrics-logs)
