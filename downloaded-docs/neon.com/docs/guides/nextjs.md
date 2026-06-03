> This page location: Frontend & Frameworks > Frameworks > Next.js
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup of a Neon project and the connection process from a Next.js application, including project creation, dependency installation, and credential management.

# Connect a Next.js application to Neon

Set up a Neon project in seconds and connect from a Next.js application

Pre-built prompt for connecting Next.js applications to Neon [View prompt](https://neon.com/prompts/nextjs-prompt.md)

Next.js by Vercel is an open-source web development framework that enables React-based web applications. This topic describes how to create a Neon project and access it from a Next.js application.

## Video walkthrough

Watch **Getting started with Neon** for an end-to-end setup with Next.js and Drizzle.

[Watch on YouTube](https://youtube.com/watch?v=XtMiMnX0hDg)

To create a Neon project and access it from a Next.js application:

## Create a Neon project

If you do not have one already, create a Neon project. Save your connection details including your password. They are required when defining connection settings.

1. Navigate to the [Projects](https://console.neon.tech/app/projects) page in the Neon Console.
2. Click **New Project**.
3. Specify your project settings and click **Create Project**.

## Create a Next.js project and add dependencies

1. Create a Next.js project if you do not have one. For instructions, see [Create a Next.js App](https://nextjs.org/docs/app/getting-started/installation), in the Vercel documentation.

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

Add a `.env` file to your project directory and add your Neon connection string to it. You can find your Neon database connection string by clicking the **Connect** button on your **Project Dashboard** to open the **Connect to your database** modal. For more information, see [Connect from any application](https://neon.com/docs/connect/connect-from-any-app).

```shell
DATABASE_URL="postgresql://<user>:<password>@<endpoint_hostname>.neon.tech:<port>/<dbname>?sslmode=require&channel_binding=require"
```

## Configure the Postgres client

There are multiple ways to make server side requests with Next.js. See below for the different implementations.

### App Router

There are two methods for fetching and mutating data using server-side requests in Next.js App Router, they are:

1. `Server Components` fetches data at runtime on the server.
2. `Server Actions` functions executed on the server to perform data mutations.

#### Server Components

In your server components using the App Router, add the following code snippet to connect to your Neon database:

**node-postgres**

```javascript
import { Pool } from 'pg';

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  ssl: true,
});

async function getData() {
  const client = await pool.connect();
  try {
    const { rows } = await client.query('SELECT version()');
    return rows[0].version;
  } finally {
    client.release();
  }
}

export default async function Page() {
  const data = await getData();
  return <>{data}</>;
}
```

**postgres.js**

```javascript
import postgres from 'postgres';

async function getData() {
  const sql = postgres(process.env.DATABASE_URL, { ssl: 'require' });
  const response = await sql`SELECT version()`;
  return response[0].version;
}

export default async function Page() {
  const data = await getData();
  return <>{data}</>;
}
```

**Neon serverless driver**

```javascript
import { neon } from '@neondatabase/serverless';

async function getData() {
  const sql = neon(process.env.DATABASE_URL);
  const response = await sql`SELECT version()`;
  return response[0].version;
}

export default async function Page() {
  const data = await getData();
  return <>{data}</>;
}
```

#### Understanding Caching in Server Components

The examples above will work in development, but in production builds, Next.js will statically render these pages at build time. This means the database query runs once during build, not on every request.

If you need fresh data on each request, add this to your page:

```typescript
export const dynamic = 'force-dynamic';
```

For other scenarios like periodic updates, see [Time-based Revalidation](https://nextjs.org/docs/app/building-your-application/caching#time-based-revalidation) in the Next.js docs.

#### Server Actions

In your server actions using the App Router, add the following code snippet to connect to your Neon database:

**node-postgres**

```javascript
import { Pool } from 'pg';

export default async function Page() {
  async function create(formData: FormData) {
    "use server";
    const pool = new Pool({
      connectionString: process.env.DATABASE_URL,
      ssl: true
    });
    const client = await pool.connect();
    await client.query("CREATE TABLE IF NOT EXISTS comments (comment TEXT)");
    const comment = formData.get("comment");
    await client.query("INSERT INTO comments (comment) VALUES ($1)", [comment]);
  }
  return (
    <form action={create}>
      <input type="text" placeholder="write a comment" name="comment" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**postgres.js**

```javascript
import postgres from 'postgres';

export default async function Page() {
  async function create(formData: FormData) {
    "use server";
    const sql = postgres(process.env.DATABASE_URL, { ssl: 'require' });
    await sql`CREATE TABLE IF NOT EXISTS comments (comment TEXT)`;
    const comment = formData.get("comment");
    await sql`INSERT INTO comments (comment) VALUES (${comment})`;
  }
  return (
    <form action={create}>
      <input type="text" placeholder="write a comment" name="comment" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Neon serverless driver**

```javascript
import { neon } from '@neondatabase/serverless';

export default async function Page() {
  async function create(formData: FormData) {
    "use server";
    const sql = neon(process.env.DATABASE_URL);
    await sql`CREATE TABLE IF NOT EXISTS comments (comment TEXT)`;
    const comment = formData.get("comment");
    await sql`INSERT INTO comments (comment) VALUES (${comment})`;
  }
  return (
    <form action={create}>
      <input type="text" placeholder="write a comment" name="comment" />
      <button type="submit">Submit</button>
    </form>
  );
}

```

### Pages Router

There are two methods for fetching data using server-side requests in Next.js Pages Router, they are:

1. `getServerSideProps` fetches data at runtime so that content is always fresh.
2. `getStaticProps` pre-renders pages at build time for data that is static or changes infrequently.

#### getServerSideProps

From `getServerSideProps` using the Pages Router, add the following code snippet to connect to your Neon database:

**node-postgres**

```javascript
import { Pool } from 'pg';

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  ssl: true,
});

export async function getServerSideProps() {
  const client = await pool.connect();
  try {
    const response = await client.query('SELECT version()');
    return { props: { data: response.rows[0].version } };
  } finally {
    client.release();
  }
}

export default function Page({ data }) {
  return <>{data}</>;
}
```

**postgres.js**

```javascript
import postgres from 'postgres';

export async function getServerSideProps() {
  const sql = postgres(process.env.DATABASE_URL, { ssl: 'require' });
  const response = await sql`SELECT version()`;
  return { props: { data: response[0].version } };
}

export default function Page({ data }) {
  return <>{data}</>;
}
```

**Neon serverless driver**

```javascript
import { neon } from '@neondatabase/serverless';

export async function getServerSideProps() {
  const sql = neon(process.env.DATABASE_URL);
  const response = await sql`SELECT version()`;
  return { props: { data: response[0].version } };
}

export default function Page({ data }) {
  return <>{data}</>;
}
```

#### getStaticProps

From `getStaticProps` using the Pages Router, add the following code snippet to connect to your Neon database:

**node-postgres**

```javascript
import { Pool } from 'pg';

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  ssl: true,
});

export async function getStaticProps() {
  const client = await pool.connect();
  try {
    const response = await client.query('SELECT version()');
    return { props: { data: response.rows[0].version } };
  } finally {
    client.release();
  }
}

export default function Page({ data }) {
  return <>{data}</>;
}
```

**postgres.js**

```javascript
import postgres from 'postgres';

export async function getStaticProps() {
  const sql = postgres(process.env.DATABASE_URL, { ssl: 'require' });
  const response = await sql`SELECT version()`;
  return { props: { data: response[0].version } };
}

export default function Page({ data }) {
  return <>{data}</>;
}
```

**Neon serverless driver**

```javascript
import { neon } from '@neondatabase/serverless';

export async function getStaticProps() {
  const sql = neon(process.env.DATABASE_URL);
  const response = await sql`SELECT version()`;
  return { props: { data: response[0].version } };
}

export default function Page({ data }) {
  return <>{data}</>;
}
```

### Serverless Functions

From your Serverless Functions, add the following code snippet to connect to your Neon database:

**node-postgres**

```javascript
import { Pool } from 'pg';

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  ssl: true,
});

export default async function handler(req, res) {
  const client = await pool.connect();
  try {
    const { rows } = await client.query('SELECT version()');
    const { version } = rows[0];
    res.status(200).json({ version });
  } finally {
    client.release();
  }
}
```

**postgres.js**

```javascript
import postgres from 'postgres';

const sql = postgres(process.env.DATABASE_URL, { ssl: 'require' });

export default async function handler(req, res) {
  const response = await sql`SELECT version()`;
  const { version } = response[0];
  res.status(200).json({ version });
}
```

**Neon serverless driver**

```javascript
import { neon } from '@neondatabase/serverless';

const sql = neon(process.env.DATABASE_URL);

export default async function handler(req, res) {
  const response = await sql`SELECT version()`;
  const { version } = response[0];
  res.status(200).json({ version });
}
```

### Edge Functions

From your Edge Functions, add the following code snippet and connect to your Neon database using the [Neon serverless driver](https://neon.com/docs/serverless/serverless-driver):

```javascript
export const config = {
  runtime: 'edge',
};

import { neon } from '@neondatabase/serverless';

const sql = neon(process.env.DATABASE_URL);

export default async function handler(req: Request) {
  const response = await sql`SELECT version()`;
  const { version } = response[0];
  return Response.json({ version });
}
```

## Run the app

When you run `npm run dev` you can expect to see the following on `localhost:3000`:

```shell
PostgreSQL 17.7 on aarch64-unknown-linux-gnu, compiled by gcc (Debian 12.2.0-14+deb12u1) 12.2.0, 64-bit
```

### Where to upload and serve files?

Neon does not provide a built-in file storage service. For managing binary file data (blobs), we recommend using dedicated, specialized storage services. Follow our guide on [File Storage](https://neon.com/docs/guides/file-storage) to learn more about how to store files in external object storage and file management services and track metadata in Neon.

## Next steps

- [Set up Neon Auth](https://neon.com/docs/auth/quick-start/nextjs-api-only): Add managed authentication that branches with your database

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
