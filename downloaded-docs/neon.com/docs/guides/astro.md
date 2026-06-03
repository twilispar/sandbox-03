> This page location: Frontend & Frameworks > Frameworks > Astro
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Step-by-step guide for connecting an Astro project to a Neon Postgres database, including project creation, dependency installation, and enabling on-demand rendering for real-time queries.

# Connect Astro to Postgres on Neon

Learn how to make server-side queries to Postgres from .astro files or API routes.

Pre-built prompt for connecting Astro to Neon using the Neon serverless driver [View prompt](https://neon.com/prompts/astro-serverless-prompt.md)

Astro builds fast content sites, powerful web applications, dynamic server APIs, and everything in-between. This guide describes how to create a Neon Postgres database and access it from an Astro site or application.

To create a Neon project and access it from an Astro site or application:

## Create a Neon project

If you do not have one already, create a Neon project. Save your connection details including your password. They are required when defining connection settings.

1. Navigate to the [Projects](https://console.neon.tech/app/projects) page in the Neon Console.
2. Click **New Project**.
3. Specify your project settings and click **Create Project**.

## Create an Astro project and add dependencies

1. Create an Astro project if you do not have one. For instructions, see [Getting Started](https://docs.astro.build/en/getting-started/), in the Astro documentation.

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

## Enable on-demand rendering

Astro requires an adapter to query databases at request time. Run:

```shell
npx astro add node
```

This enables [on-demand rendering](https://docs.astro.build/en/guides/on-demand-rendering/) (also known as server-side rendering or SSR) so your pages can fetch fresh data on each request.

**Note:** This adapter is required for real-time database queries in production. Without it, your database is queried only once at build time, and production will serve static data. Development mode (`npm run dev`) rebuilds pages automatically and does not require the adapter.

## Store your Neon credentials

Add a `.env` file to your project directory and add your Neon connection string to it. You can find the connection string for your database by clicking the **Connect** button on your **Project Dashboard** to open the **Connect to your database** modal. For more information, see [Connect from any application](https://neon.com/docs/connect/connect-from-any-app).

```shell
DATABASE_URL="postgresql://<user>:<password>@<endpoint_hostname>.neon.tech:<port>/<dbname>?sslmode=require&channel_binding=require"
```

## Create a database utility

Create a reusable database client that you can import throughout your application. This approach centralizes your database configuration and follows best practices for code organization.

Create a new file at `src/lib/neon.ts` (or `src/lib/neon.js`) with the following code:

**node-postgres**

```typescript
import { Pool } from 'pg';

export const pool = new Pool({
  connectionString: import.meta.env.DATABASE_URL,
  ssl: true
});
```

**postgres.js**

```typescript
import postgres from 'postgres';

export const sql = postgres(import.meta.env.DATABASE_URL, { ssl: 'require' });
```

**Neon serverless driver**

```typescript
import { neon } from '@neondatabase/serverless';

export const sql = neon(import.meta.env.DATABASE_URL);
```

## Configure the Postgres client

There are multiple ways to make server side requests with Astro. See below for two of those options: **Page Components (.astro files)** and **Server Endpoints (API Routes)**.

### Page Components (.astro files)

In your `.astro` page components (for example, `src/pages/index.astro`), you can query the database in the frontmatter section (between the `---` fences). Import the database client from your utility file:

**node-postgres**

```astro
---
import { pool } from '../lib/neon';

const client = await pool.connect();

let data = null;

try {
  const response = await client.query('SELECT version()');
  data = response.rows[0].version;
} finally {
  client.release();
}
---

{data}
```

**postgres.js**

```astro
---
import { sql } from '../lib/neon';

const response = await sql`SELECT version()`;
const data = response[0].version;
---

{data}
```

**Neon serverless driver**

```astro
---
import { sql } from '../lib/neon';

const response = await sql`SELECT version()`;
const data = response[0].version;
---

{data}
```

**Note:** You can also initialize the database connection directly in each `.astro` file, but using a shared utility file is recommended for maintainability and code organization.

#### Run the app

When you run `npm run dev` you can expect to see something like the following when you visit `localhost:4321`:

```shell
PostgreSQL 17.7 on aarch64-unknown-linux-gnu, compiled by gcc (Debian 12.2.0-14+deb12u1) 12.2.0, 64-bit
```

### Server Endpoints (API Routes)

In your server endpoints (API Routes) in Astro application, import the database client from your utility file:

**node-postgres**

```javascript
// File: src/pages/api/index.ts

import { pool } from '../../lib/neon';

export async function GET() {
  const client = await pool.connect();
  let data = {};
  try {
    const { rows } = await client.query('SELECT version()');
    data = rows[0];
  } finally {
    client.release();
  }
  return new Response(JSON.stringify(data), { headers: { 'Content-Type': 'application/json' } });
}
```

**postgres.js**

```javascript
// File: src/pages/api/index.ts

import { sql } from '../../lib/neon';

export async function GET() {
  const response = await sql`SELECT version()`;
  return new Response(JSON.stringify(response[0]), {
    headers: { 'Content-Type': 'application/json' },
  });
}
```

**Neon serverless driver**

```javascript
// File: src/pages/api/index.ts

import { sql } from '../../lib/neon';

export async function GET() {
  const response = await sql`SELECT version()`;
  return new Response(JSON.stringify(response[0]), {
    headers: { 'Content-Type': 'application/json' },
  });
}
```

#### Run the app

When you run `npm run dev` you can expect to see something like the following when you visit the `localhost:4321/api` route:

```shell
{ version: 'PostgreSQL 17.7 on aarch64-unknown-linux-gnu, compiled by gcc (Debian 12.2.0-14+deb12u1) 12.2.0, 64-bit' }
```

---

## Related docs (Frameworks)

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
- [Sveltekit](https://neon.com/docs/guides/sveltekit)
- [Symfony](https://neon.com/docs/guides/symfony)
- [Hono](https://neon.com/docs/guides/hono)
- [RedwoodSDK](https://neon.com/docs/guides/redwoodsdk)
- [Vue](https://neon.com/docs/guides/vue)
