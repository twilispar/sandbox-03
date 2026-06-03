> This page location: Frontend & Frameworks > ORMs > Knex
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: How to connect Knex to Neon, including steps for establishing a connection, configuring environment variables, and utilizing connection pooling for optimal performance in serverless applications.

# Connect from Knex to Neon

Learn how to connect to Neon from Knex

Knex is an open-source SQL query builder for Postgres. This guide covers the following topics:

- [Connect to Neon from Knex](https://neon.com/docs/guides/knex#connect-to-neon-from-knex)
- [Use connection pooling with Knex](https://neon.com/docs/guides/knex#use-connection-pooling-with-knex)
- [Performance tips](https://neon.com/docs/guides/knex#performance-tips)

## Connect to Neon from Knex

To establish a basic connection from Knex to Neon, perform the following steps:

1. Find your database connection string by clicking the **Connect** button on your **Project Dashboard** to open the **Connect to your database** modal. Select a branch, a user, and the database you want to connect to. A connection string is constructed for you.
   ![Connection details modal](https://neon.com/docs/connect/connection_details.png)
   The connection string includes the user name, password, hostname, and database name.

2. Update the Knex's initialization in your application to the following:

   ```typescript {2-5}
   export const client = knex({
     client: 'pg',
     connection: {
       connectionString: process.env.DATABASE_URL,
     },
   });
   ```

3. Add a `DATABASE_URL` variable to your `.env` file and set it to the Neon connection string that you copied in the previous step. We also recommend adding `?sslmode=require&channel_binding=require` to the end of the connection string to ensure a [secure connection](https://neon.com/docs/connect/connect-securely).

   Your setting will appear similar to the following:

   ```text
   DATABASE_URL="postgresql://[user]:[password]@[neon_hostname]/[dbname]?sslmode=require&channel_binding=require"
   ```

## Use connection pooling with Knex

Serverless functions can require a large number of database connections as demand increases. If you use serverless functions in your application, we recommend that you use a pooled Neon connection string, as shown:

```ini
# Pooled Neon connection string
DATABASE_URL="postgresql://alex:AbC123dEf@ep-cool-darkness-123456-pooler.us-east-2.aws.neon.tech/dbname?sslmode=require&channel_binding=require"
```

A pooled Neon connection string adds `-pooler` to the endpoint ID, which tells Neon to use a pooled connection. You can add `-pooler` to your connection string manually or copy a pooled connection string by clicking the **Connect** button on your **Project Dashboard** to open the **Connect to your database** modal. Enable the **Connection pooling** toggle to add the `-pooler` suffix.

## Performance tips

This section outlines performance optimizations you can try when using Knex with Neon.

### Enabling NODE_PG_FORCE_NATIVE

Knex leverages a [node-postgres](https://node-postgres.com) Pool instance to connect to your Postgres database. Installing [pg-native](https://npmjs.com/package/pg-native) and setting the `NODE_PG_FORCE_NATIVE` environment variable to `true` [switches the `pg` driver to `pg-native`](https://github.com/brianc/node-postgres/blob/master/packages/pg/lib/index.js#L31-L34), which can produce noticeably faster response times according to some users.

### Replacing query parameters

You may be able to achieve better performance with Knex by replacing any parameters you've defined in your queries, as performed by the following function, for example:

```tsx
// Function to replace query parameters in a query
function replaceQueryParams(query, values) {
  let replacedQuery = query;
  values.forEach((tmpParameter) => {
    if (typeof tmpParameter === 'string') {
      replacedQuery = replacedQuery.replace('?', `'${tmpParameter}'`);
    } else {
      replacedQuery = replacedQuery.replace('?', tmpParameter);
    }
  });
  return replacedQuery;
}

// So instead of this
await client.raw(text, values);

// Do this to get better performance
await client.raw(replaceQueryParams(text, values));
```

---

## Related docs (ORMs)

- [Django (Django ORM)](https://neon.com/docs/guides/django)
- [Drizzle](https://neon.com/docs/guides/drizzle)
- [Elixir Ecto](https://neon.com/docs/guides/elixir-ecto)
- [Kysely](https://neon.com/docs/guides/kysely)
- [Laravel (Eloquent)](https://neon.com/docs/guides/laravel)
- [Prisma](https://neon.com/docs/guides/prisma)
- [Ruby on Rails (ActiveRecord)](https://neon.com/docs/guides/ruby-on-rails)
- [SQLAlchemy](https://neon.com/docs/guides/sqlalchemy)
- [Tortoise ORM](https://neon.com/docs/guides/tortoise-orm)
- [TypeORM](https://neon.com/docs/guides/typeorm)
