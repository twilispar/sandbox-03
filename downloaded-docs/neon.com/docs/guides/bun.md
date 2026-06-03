> This page location: Frontend & Frameworks > Frameworks > Bun
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Step-by-step guide for creating a Neon project and connecting it to a Bun application, including examples for using Bun's SQL client and the Neon serverless driver.

# Connect a Bun application to Neon

Set up a Neon project in seconds and connect from a Bun application

This guide describes how to create a Neon project and connect to it from a Bun application. Examples are provided for using [Bun's built-in SQL client](https://bun.sh/docs/api/sql) and the [@neondatabase/serverless](https://neon.com/docs/serverless/serverless-driver) driver. Use the client you prefer.

**Note:** The same configuration steps can be used for [Hono](https://hono.dev/docs/getting-started/bun), [Elysia](https://elysiajs.com), and other Bun-based web frameworks.

## Create a Neon project

If you do not have one already, create a Neon project.

1. Navigate to the [Projects](https://console.neon.tech/app/projects) page in the Neon Console.
2. Click **New Project**.
3. Specify your project settings and click **Create Project**.

## Create a Bun project and add dependencies

Create a Bun project and change to the newly created directory:

```shell
mkdir bun-neon-example
cd bun-neon-example
bun init -y
```

Next, add project dependencies if you intend to use the Neon serverless driver. Otherwise, Bun's built-in `sql` client is readily available.

**Bun.sql**

```shell
# No dependencies needed for Bun's built-in SQL client
```

**Neon serverless driver**

```shell
bun add @neondatabase/serverless
```

## Store your Neon credentials

Add a `.env.local` file to your project directory and add your Neon connection details to it. Bun automatically loads variables from `.env`, `.env.local`, and other `.env.*` files. You can find the connection details for your database by clicking the **Connect** button on your **Project Dashboard** to open the **Connect to your database** modal. Select Bun from the **Connection string** dropdown. For more information, see [Connect from any application](https://neon.com/docs/connect/connect-from-any-app).

```shell
POSTGRES_URL='postgresql://[user]:[password]@[neon_hostname]/[dbname]?sslmode=require&channel_binding=require'
```

**Note:** `Bun.sql` uses `POSTGRES_URL` as the default environment variable for the Primary connection URL for Postgres

**Important:** To ensure the security of your data, never expose your Neon credentials directly in your code or commit them to version control.

## Configure the Postgres client

Add an `index.ts` file (or `index.js`) to your project directory and add the following code snippet to connect to your Neon database. Choose the configuration that matches your preferred client.

**Bun.sql**

```typescript
import { sql } from 'bun';

async function getPgVersion() {
  const result = await sql`SELECT version()`;
  console.log(result[0]);
}

getPgVersion();
```

**Neon serverless driver**

```typescript
import { neon } from '@neondatabase/serverless';

const sql = neon(process.env.POSTGRES_URL);

async function getPgVersion() {
  const result = await sql`SELECT version()`;
  console.log(result[0]);
}

getPgVersion();
```

## Run index.ts

Run `bun run index.ts` (or `bun index.js`) to view the result.

```shell
$ bun run index.ts
{
  version: "PostgreSQL 17.2 on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit",
}
```

## References

- [Bun SQL client](https://bun.sh/docs/api/sql)
- [@neondatabase/serverless driver](https://neon.com/docs/serverless/serverless-driver)

---

## Related docs (Frameworks)

- [Astro](https://neon.com/docs/guides/astro)
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
