> This page location: Frontend & Frameworks > Frameworks > Sveltekit
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Step-by-step guide for connecting a SvelteKit application to a Neon project, including project creation, dependency installation, and configuration of database credentials.

# Connect a Sveltekit application to Neon

Set up a Neon project in seconds and connect from a Sveltekit application

Pre-built prompt for connecting SvelteKit applications to Neon. [View prompt](https://neon.com/prompts/sveltekit-prompt.md)

Sveltekit is a modern JavaScript framework that compiles your code to tiny, framework-less vanilla JS. This guide explains how to connect Sveltekit with Neon using a secure server-side request.

To create a Neon project and access it from a Sveltekit application:

## Create a Neon project

If you do not have one already, create a Neon project. Save your connection details including your password. They are required when defining connection settings.

1. Navigate to the [Projects](https://console.neon.tech/app/projects) page in the Neon Console.
2. Click **New Project**.
3. Specify your project settings and click **Create Project**.

## Create a Sveltekit project and add dependencies

1. Create a Sveltekit project using the following commands:

   ```shell
   npx sv create my-app --template minimal --no-add-ons --types ts
   cd my-app
   ```

2. Add project dependencies using one of the following commands:

   **node-postgres**

   ```shell
   npm install pg dotenv
   ```

   **postgres.js**

   ```shell
   npm install postgres dotenv
   ```

   **Neon serverless driver**

   ```shell
   npm install @neondatabase/serverless dotenv
   ```

## Store your Neon credentials

Add a `.env` file to your project directory and add your Neon connection string to it. You can find the connection string for your database by clicking the **Connect** button on your **Project Dashboard**. For more information, see [Connect from any application](https://neon.com/docs/connect/connect-from-any-app).

```shell
DATABASE_URL="postgresql://<user>:<password>@<endpoint_hostname>.neon.tech:<port>/<dbname>?sslmode=require&channel_binding=require"
```

## Configure the Postgres client

There are two parts to connecting a SvelteKit application to Neon. The first is `db.server.ts`, which contains the database configuration. The second is the server-side route where the connection to the database will be used.

### db.server

Create a `db.server.ts` file at the root of your `/src` directory and add the following code snippet to connect to your Neon database:

**node-postgres**

```typescript
import 'dotenv/config';
import pg from 'pg';

const connectionString: string = process.env.DATABASE_URL as string;

const pool = new pg.Pool({
  connectionString,
  ssl: true,
});

export { pool };
```

**postgres.js**

```typescript
import 'dotenv/config';
import postgres from 'postgres';

const connectionString: string = process.env.DATABASE_URL as string;

const sql = postgres(connectionString, { ssl: 'require' });

export { sql };
```

**Neon serverless driver**

```typescript
import 'dotenv/config';
import { neon } from '@neondatabase/serverless';

const connectionString: string = process.env.DATABASE_URL as string;

const sql = neon(connectionString);
export { sql };
```

### route

Create a `+page.server.ts` file in your route directory and import the database configuration:

**node-postgres**

```typescript
import { pool } from '../db.server';

export async function load() {
  const client = await pool.connect();
  try {
    const { rows } = await client.query('SELECT version()');
    const { version } = rows[0];
    return {
      version,
    };
  } finally {
    client.release();
  }
}
```

**postgres.js**

```typescript
import { sql } from '../db.server';

export async function load() {
  const response = await sql`SELECT version()`;
  const { version } = response[0];
  return {
    version,
  };
}
```

**Neon serverless driver**

```typescript
import { sql } from '../db.server';

export async function load() {
  const response = await sql`SELECT version()`;
  const { version } = response[0];
  return {
    version,
  };
}
```

### Page Component

Create a `+page.svelte` file to display the data:

```svelte
<script>
    export let data;
</script>

<h1>Database Version</h1>
<p>{data.version}</p>
```

## Run the app

When you run `npm run dev` you can expect to see the following on [localhost:5173](https://neon.com/docs/guides/localhost:5173):

```shell
Database Version
PostgreSQL 17.2 on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
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
- [Remix](https://neon.com/docs/guides/remix)
- [SolidStart](https://neon.com/docs/guides/solid-start)
- [Symfony](https://neon.com/docs/guides/symfony)
- [Hono](https://neon.com/docs/guides/hono)
- [RedwoodSDK](https://neon.com/docs/guides/redwoodsdk)
- [Vue](https://neon.com/docs/guides/vue)
