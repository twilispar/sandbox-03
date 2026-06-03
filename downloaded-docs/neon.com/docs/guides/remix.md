> This page location: Frontend & Frameworks > Frameworks > Remix
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: How to connect a Remix application to a Neon project by creating a Neon project, setting up a Remix project, and configuring connection settings securely.

# Connect a Remix application to Neon

Set up a Neon project in seconds and connect from a Remix application

**Note:** Remix is now React Router v7. The features of the Remix framework have been merged into React Router v7. If you are starting a new project, we recommend using React Router. Follow our [React Router guide](https://neon.com/docs/guides/react-router) to connect to Neon.

For more information, see the [Remix announcement](https://remix.run/blog/merging-remix-and-react-router).

Remix is an open-source full stack JavaScript framework that lets you focus on building out the user interface using familiar web standards. This guide explains how to connect Remix with Neon using a secure server-side request.

To create a Neon project and access it from a Remix application:

## Create a Neon project

If you do not have one already, create a Neon project. Save your connection details including your password. They are required when defining connection settings.

1. Navigate to the [Projects](https://console.neon.tech/app/projects) page in the Neon Console.
2. Click **New Project**.
3. Specify your project settings and click **Create Project**.

## Create a Remix project and add dependencies

1. Create a Remix project if you do not have one. For instructions, see [Quick Start](https://remix.run/docs/en/main/start/quickstart), in the Remix documentation.

2. Add project dependencies using one of the following commands:

   **node-postgres**

   ```shell
   npm install pg
   ```

   **postgres.js**

   ```shell
   npm install postgres
   ```

   **Neon serverless driver**

   ```shell
   npm install @neondatabase/serverless
   ```

## Store your Neon credentials

Add a `.env` file to your project directory and add your Neon connection string to it. You can find the connection string for your database by clicking the **Connect** button on your **Project Dashboard**. For more information, see [Connect from any application](https://neon.com/docs/connect/connect-from-any-app).

```shell
DATABASE_URL="postgresql://<user>:<password>@<endpoint_hostname>.neon.tech:<port>/<dbname>?sslmode=require&channel_binding=require"
```

## Configure the Postgres client

There are two parts to connecting a Remix application to Neon. The first is `db.server`. Remix will ensure any code added to this file won't be included in the client bundle. The second is the route where the connection to the database will be used.

### db.server

Create a `db.server.ts` file at the root of your `/app` directory and add the following code snippet to connect to your Neon database:

**node-postgres**

```javascript
import pg from 'pg';

const pool = new pg.Pool({
  connectionString: process.env.DATABASE_URL,
  ssl: true,
});

export { pool };
```

**postgres.js**

```javascript
import postgres from 'postgres';

const sql = postgres(process.env.DATABASE_URL, { ssl: 'require' });

export { sql };
```

**Neon serverless driver**

```javascript
import { neon } from '@neondatabase/serverless';

const sql = neon(process.env.DATABASE_URL);

export { sql };
```

### route

Create a new route in your `app/routes` directory and import the `db.server` file.

**node-postgres**

```javascript
import { pool } from '~/db.server';
import { json } from '@remix-run/node';
import { useLoaderData } from '@remix-run/react';

export const loader = async () => {
  const client = await pool.connect();
  try {
    const response = await client.query('SELECT version()');
    return response.rows[0].version;
  } finally {
    client.release();
  }
};

export default function Page() {
  const data = useLoaderData();
  return <>{data}</>;
}
```

**postgres.js**

```javascript
import { sql } from '~/db.server';
import { json } from '@remix-run/node';
import { useLoaderData } from '@remix-run/react';

export const loader = async () => {
  const response = await sql`SELECT version()`;
  return response[0].version;
};

export default function Page() {
  const data = useLoaderData();
  return <>{data}</>;
}
```

**Neon serverless driver**

```javascript
import { sql } from '~/db.server';
import { json } from '@remix-run/node';
import { useLoaderData } from '@remix-run/react';

export const loader = async () => {
  const response = await sql`SELECT version()`;
  return response[0].version;
};

export default function Page() {
  const data = useLoaderData();
  return <>{data}</>;
}
```

## Run the app

When you run `npm run dev` you can expect to see the following on [localhost:3000](https://neon.com/docs/guides/localhost:3000):

```shell
PostgreSQL 16.0 on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
```

---

## Related docs (Frameworks)

- [Astro](https://neon.com/docs/guides/astro)
- [Bun](https://neon.com/docs/guides/bun)
- [Entity Framework](https://neon.com/docs/guides/dotnet-entity-framework)
- [Encore](https://neon.com/docs/guides/encore)
- [Express](https://neon.com/docs/guides/express)
- [Medusa.js](https://neon.com/docs/guides/medusajs)
- [Micronaut Kotlin](https://neon.com/docs/guides/micronaut-kotlin)
- [NestJS](https://neon.com/docs/guides/nestjs)
- [Next.js](https://neon.com/docs/guides/nextjs)
- [Node.js](https://neon.com/docs/guides/node)
- [Nuxt](https://neon.com/docs/guides/nuxt)
- [Phoenix](https://neon.com/docs/guides/phoenix)
- [Quarkus (JDBC)](https://neon.com/docs/guides/quarkus-jdbc)
- [Quarkus (Reactive)](https://neon.com/docs/guides/quarkus-reactive)
- [React](https://neon.com/docs/guides/react)
- [React Router](https://neon.com/docs/guides/react-router)
- [Reflex](https://neon.com/docs/guides/reflex)
- [SolidStart](https://neon.com/docs/guides/solid-start)
- [Sveltekit](https://neon.com/docs/guides/sveltekit)
- [Symfony](https://neon.com/docs/guides/symfony)
- [Hono](https://neon.com/docs/guides/hono)
- [RedwoodSDK](https://neon.com/docs/guides/redwoodsdk)
- [Vue](https://neon.com/docs/guides/vue)
