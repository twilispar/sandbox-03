> This page location: Frontend & Frameworks > Frameworks > Express
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Step-by-step guide for creating a Neon project and connecting it to an Express application using various PostgreSQL clients, including the Neon serverless driver, node-postgres, and Postgres.js.

# Connect an Express application to Neon

Set up a Neon project in seconds and connect from an Express application

Pre-built prompt for connecting ExpressJS applications to Neon Postgres [View prompt](https://neon.com/prompts/express-prompt.md)

This guide describes how to create a Neon project and connect to it from an Express application.

## Choose a driver

- **`@neondatabase/serverless`** connects over HTTP or WebSockets instead of TCP. Use it for serverless and edge platforms without built-in connection pooling (Vercel, Netlify Functions, Deno Deploy, Cloudflare Workers without Hyperdrive).
- **`postgres` (postgres.js)** is a fast, full-featured client for both serverless and traditional Node.js server environments.
- **`pg` (node-postgres)** is the classic, widely-used driver for traditional, long-running Node.js servers. Also the standard choice for [Vercel Fluid compute](https://vercel.com/docs/functions/fluid-compute) and Cloudflare with [Hyperdrive](https://developers.cloudflare.com/hyperdrive/).

For a detailed comparison including all platforms, see [Choosing your connection method](https://neon.com/docs/connect/choose-connection).

## Create a Neon project

If you do not have one already, create a Neon project.

1. Navigate to the [Projects](https://console.neon.tech/app/projects) page in the Neon Console.
2. Click **New Project**.
3. Specify your project settings and click **Create Project**.

## Create an Express project and add dependencies

1. Create an Express project and change to the newly created directory.

   ```shell
   mkdir neon-express-example
   cd neon-express-example
   npm init -y
   npm install express
   ```

2. Add project dependencies using one of the following commands:

   **Neon serverless driver**

   ```shell
   npm install @neondatabase/serverless dotenv
   ```

   **node-postgres**

   ```shell
   npm install pg dotenv
   ```

   **postgres.js**

   ```shell
   npm install postgres dotenv
   ```

## Store your Neon credentials

Add a `.env` file to your project directory and add your Neon connection details to it. Find your database connection details by clicking the **Connect** button on your **Project Dashboard** to open the **Connect to your database** modal. Select Node.js from the **Connection string** dropdown. For more information, see [Connect from any application](https://neon.com/docs/connect/connect-from-any-app).

```shell
DATABASE_URL="postgresql://<user>:<password>@<endpoint_hostname>.neon.tech:<port>/<dbname>?sslmode=require&channel_binding=require"
```

**Important:** Never hardcode credentials in source code files. Always use environment variables via `process.env`. For more information, see [Security overview](https://neon.com/docs/security/security-overview).

## Configure the Postgres client

Add an `index.js` file to your project directory and add the following code snippet to connect to your Neon database:

**Neon serverless driver**

```javascript
require('dotenv').config();

const express = require('express');
const { neon } = require('@neondatabase/serverless');

const app = express();
const PORT = process.env.PORT || 4242;

const sql = neon(process.env.DATABASE_URL);

app.get('/', async (req, res) => {
  try {
    const [result] = await sql`SELECT version()`;
    const version = result?.version || 'No version found';
    res.json({ version });
  } catch (error) {
    console.error('Database query failed:', error);
    res.status(500).json({ error: 'Failed to connect to the database.' });
  }
});

app.listen(PORT, () => {
  console.log(`Listening to http://localhost:${PORT}`);
});
```

**node-postgres**

```javascript
require('dotenv').config();

const express = require('express');
const { Pool } = require('pg');

const app = express();
const PORT = process.env.PORT || 4242;

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
});

app.get('/', async (req, res) => {
  let client;
  try {
    client = await pool.connect();
    const { rows } = await client.query('SELECT version()');
    const version = rows[0]?.version || 'No version found';
    res.json({ version });
  } catch (error) {
    console.error('Database query failed:', error);
    res.status(500).json({ error: 'Failed to connect to the database.' });
  } finally {
    client?.release();
  }
});

app.listen(PORT, () => {
  console.log(`Listening to http://localhost:${PORT}`);
});
```

**postgres.js**

```javascript
require('dotenv').config();

const express = require('express');
const postgres = require('postgres');

const app = express();
const PORT = process.env.PORT || 4242;

const sql = postgres(process.env.DATABASE_URL);

app.get('/', async (req, res) => {
  try {
    const [result] = await sql`SELECT version()`;
    const version = result?.version || 'No version found';
    res.json({ version });
  } catch (error) {
    console.error('Database query failed:', error);
    res.status(500).json({ error: 'Failed to connect to the database.' });
  }
});

app.listen(PORT, () => {
  console.log(`Listening to http://localhost:${PORT}`);
});
```

## Run index.js

Run `node index.js` to view the result on [localhost:4242](https://neon.com/docs/guides/localhost:4242) as follows:

```shell
{ version: 'PostgreSQL 16.0 on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit' }
```

<details>

<summary>**Notes for AI-assisted setup**</summary>

- Do not hardcode credentials or connection strings in any `.js` source file. Always use environment variables via `process.env` and `dotenv`.
- Wrap all database queries in a `try...catch` block. Return a `500` status with an error message on failure.
- When using `pg` (node-postgres), declare `let client` before the `try` block and call `client?.release()` in the `finally` block. This safely handles cases where `pool.connect()` itself fails.
- Initialize the connection pool or client outside the route handler, not inside it. Creating a new pool on every request wastes resources.
- Choose the right driver for your platform. `@neondatabase/serverless` is for serverless/edge platforms without TCP support. For long-running Express servers, use `pg` or `postgres`. See [Choosing your connection method](https://neon.com/docs/connect/choose-connection).

</details>

---

## Related docs (Frameworks)

- [Astro](https://neon.com/docs/guides/astro)
- [Bun](https://neon.com/docs/guides/bun)
- [Entity Framework](https://neon.com/docs/guides/dotnet-entity-framework)
- [Encore](https://neon.com/docs/guides/encore)
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
- [Sveltekit](https://neon.com/docs/guides/sveltekit)
- [Symfony](https://neon.com/docs/guides/symfony)
- [Hono](https://neon.com/docs/guides/hono)
- [RedwoodSDK](https://neon.com/docs/guides/redwoodsdk)
- [Vue](https://neon.com/docs/guides/vue)
