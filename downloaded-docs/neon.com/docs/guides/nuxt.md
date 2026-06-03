> This page location: Frontend & Frameworks > Frameworks > Nuxt
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Step-by-step guide for connecting a Nuxt application to a Postgres database on Neon, including project creation, dependency installation, and configuration of connection settings.

# Connect Nuxt to Postgres on Neon

Learn how to make server-side queries to Postgres using Nitro API routes

Pre-built prompt for connecting Nuxt applications to Neon. [View prompt](https://neon.com/prompts/nuxt-neon-prompt.md)

[Nuxt](https://nuxt.com/) is an open-source full-stack meta framework that enables Vue-based web applications. This topic describes how to connect a Nuxt application to a Postgres database on Neon.

To create a Neon project and access it from a Nuxt.js application:

## Create a Neon project

If you do not have one already, create a Neon project. Save your connection details including your password. They are required when defining connection settings.

1. Navigate to the [Projects](https://console.neon.tech/app/projects) page in the Neon Console.
2. Click **New Project**.
3. Specify your project settings and click **Create Project**.

## Create a Nuxt project and add dependencies

1. Create a Nuxt project if you do not have one. For instructions, see [Create a Nuxt Project](https://nuxt.com/docs/getting-started/installation#new-project), in the Nuxt documentation.

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

Add a `.env` file to your project directory and add your Neon connection string to it. You can find your connection string by clicking the **Connect** button on your **Project Dashboard** to open the **Connect to your database** modal. For more information, see [Connect from any application](https://neon.com/docs/connect/connect-from-any-app).

```shell
NUXT_DATABASE_URL="postgresql://<user>:<password>@<endpoint_hostname>.neon.tech:<port>/<dbname>?sslmode=require&channel_binding=require"
```

## Configure the Postgres client

First, make sure you load the `NUXT_DATABASE_URL` from your .env file in Nuxt's runtime configuration:

In `nuxt.config.js`:

```javascript
export default defineNuxtConfig({
  runtimeConfig: {
    databaseUrl: '',
  },
});
```

Next, use the Neon serverless driver to create a database connection. Here's an example configuration:

```javascript
import { neon } from '@neondatabase/serverless';

export default defineCachedEventHandler(
  async (event) => {
    const { databaseUrl } = useRuntimeConfig();
    const db = neon(databaseUrl);
    const result = await db`SELECT version()`;
    return result;
  },
  {
    maxAge: 60 * 60 * 24, // cache it for a day
  }
);
```

**Note:**

- This example demonstrates using the Neon serverless driver to run a simple query. The `useRuntimeConfig` method accesses the `databaseUrl` set in your Nuxt runtime configuration.
- Async Handling: Make sure the handler is async if you are awaiting the database query result.
- Make sure `maxAge` caching fits your application's needs. In this example, it's set to cache results for a day. Adjust as necessary.

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
