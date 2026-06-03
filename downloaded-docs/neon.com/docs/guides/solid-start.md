> This page location: Frontend & Frameworks > Frameworks > SolidStart
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Step-by-step guide for connecting a SolidStart application to a Neon project, including project creation, dependency installation, and configuration of connection settings for secure server-side requests.

# Connect a SolidStart application to Neon

Set up a Neon project in seconds and connect from a SolidStart application

Pre-built prompt for connecting SolidStart applications to Neon [View prompt](https://neon.com/prompts/solidstart-prompt.md)

SolidStart is an open-source meta-framework designed to integrate the components that make up a web application.[1](https://docs.solidjs.com/solid-start#overview). This guide explains how to connect SolidStart with Neon using a secure server-side request.

To create a Neon project and access it from a SolidStart application:

## Create a Neon project

If you do not have one already, create a Neon project. Save your connection details including your password. They are required when defining connection settings.

1. Navigate to the [Projects](https://console.neon.tech/app/projects) page in the Neon Console.
2. Click **New Project**.
3. Specify your project settings and click **Create Project**.

## Create a SolidStart project and add dependencies

1. Create a SolidStart project if you do not have one. For instructions, see [Quick Start](https://docs.solidjs.com/solid-start/getting-started), in the SolidStart documentation.

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

There a multiple ways to make server-side requests with SolidStart. See below for the different implementations.

### Server-Side Data Loading

To [load data on the server](https://docs.solidjs.com/solid-start/building-your-application/data-loading#data-loading-always-on-the-server) in SolidStart, add the following code snippet to connect to your Neon database:

**node-postgres**

```typescript
import pg from 'pg';
import { createAsync, query } from "@solidjs/router";

const getVersion = query(async () => {
    "use server";
    const pool = new pg.Pool({
        connectionString: process.env.DATABASE_URL,
    });
    const client = await pool.connect();
    const response = await client.query('SELECT version()');
    return response.rows[0].version;
}, 'version')

export const route = {
  preload: () => getVersion(),
};

export default function Page() {
  const version = createAsync(() => getVersion());
  return <>{version()}</>;
}
```

**postgres.js**

```typescript
import postgres from 'postgres';
import { createAsync, query } from "@solidjs/router";

const getVersion = query(async () => {
    "use server";
    const sql = postgres(process.env.DATABASE_URL, { ssl: 'require' });
    const response = await sql`SELECT version()`;
    return response[0].version;
}, 'version')

export const route = {
  preload: () => getVersion(),
};

export default function Page() {
  const version = createAsync(() => getVersion());
  return <>{version()}</>;
}
```

**Neon serverless driver**

```typescript
import { neon } from "@neondatabase/serverless";
import { createAsync, query } from "@solidjs/router";

const getVersion = query(async () => {
    "use server";
    const sql = neon(process.env.DATABASE_URL);
    const response = await sql`SELECT version()`;
    const { version } = response[0];

    return version;
}, 'version')

export const route = {
  preload: () => getVersion(),
};

export default function Page() {
  const version = createAsync(() => getVersion());
  return <>{version()}</>;
}
```

### Server Endpoints (API Routes)

In your server endpoints (API Routes) in your SolidStart application, use the following code snippet to connect to your Neon database:

**node-postgres**

```javascript
// File: routes/api/test.ts

import { Pool } from 'pg';
import { json } from '@solidjs/router';

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  ssl: true,
});

export async function GET() {
  const client = await pool.connect();
  let data = {};
  try {
    const { rows } = await client.query('SELECT version()');
    data = rows[0];
  } finally {
    client.release();
  }
  return json(data);
}
```

**postgres.js**

```javascript
// File: routes/api/test.ts

import postgres from 'postgres';
import { json } from '@solidjs/router';

export async function GET() {
  const sql = postgres(process.env.DATABASE_URL, { ssl: 'require' });
  const response = await sql`SELECT version()`;

  return json(response[0]);
}
```

**Neon serverless driver**

```javascript
// File: routes/api/test.ts

import { neon } from '@neondatabase/serverless';
import { json } from '@solidjs/router'

export async function GET() {
  const sql = neon(process.env.DATABASE_URL!);
  const response = await sql`SELECT version()`;

  return json(response[0]);
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
- [Remix](https://neon.com/docs/guides/remix)
- [Sveltekit](https://neon.com/docs/guides/sveltekit)
- [Symfony](https://neon.com/docs/guides/symfony)
- [Hono](https://neon.com/docs/guides/hono)
- [RedwoodSDK](https://neon.com/docs/guides/redwoodsdk)
- [Vue](https://neon.com/docs/guides/vue)
