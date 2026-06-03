> This page location: Frontend & Frameworks > Frameworks > RedwoodSDK
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Step-by-step guide for creating a Neon project and connecting it to a RedwoodSDK application, including setting up dependencies and storing connection credentials.

# Connect a RedwoodSDK application to Neon

Set up a Neon project in seconds and connect from a Redwood application

Pre-built prompt for connecting RedwoodSDK applications to Neon Postgres [View prompt](https://neon.com/prompts/redwood-sdk-prompt.md)

[RedwoodSDK](https://rwsdk.com/) is a framework for building full-stack applications on Cloudflare. This guide describes how to create a Neon project and access it from a RedwoodSDK application.

To create a Neon project and access it from a RedwoodSDK application:

## Create a Neon project

If you do not have one already, create a Neon project. Save your connection details including your password. They are required when defining connection settings.

1. Navigate to the [Projects](https://console.neon.tech/app/projects) page in the Neon Console.
2. Click **New Project**.
3. Specify your project settings and click **Create Project**.

## Create a RedwoodSDK project and add dependencies

1. Create a RedwoodSDK project if you do not have one. For instructions, see [RedwoodSDK Quickstart](https://docs.rwsdk.com/getting-started/quick-start/).

2. Navigate into your new project directory and install the RedwoodSDK dependencies:

   ```bash
   cd my-redwood-app
   npm install
   ```

3. Add project dependencies depending on the PostgreSQL driver you wish to use (`postgres.js` or `@neondatabase/serverless`):

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

In your RedwoodSDK application (for example, in `src/app/pages/Home.tsx`), import the driver and use it within your route handlers.

Here's how you can set up a simple route to query the database:

**postgres.js**

```typescript
import { RequestInfo } from "rwsdk/worker";
import postgres from 'postgres';
import { env } from "cloudflare:workers";

async function getData() {
  const sql = postgres(env.DATABASE_URL, { ssl: 'require' });
  const response = await sql`SELECT version()`;
  return response[0].version;
}

export async function Home({ ctx }: RequestInfo) {
  const data = await getData();
  return <>{data}</>;
}
```

**Neon serverless driver**

```typescript
import { RequestInfo } from "rwsdk/worker";
import { neon } from '@neondatabase/serverless';
import { env } from "cloudflare:workers";

async function getData() {
  const sql = neon(env.DATABASE_URL);
  const response = await sql`SELECT version()`;
  return response[0].version;
}

export async function Home({ ctx }: RequestInfo) {
  const data = await getData();
  return <>{data}</>;
}
```

## Run your RedwoodSDK application

Generate the required Wrangler types for RedwoodSDK to detect environment variables:

```bash
npx wrangler types
```

Start the development server:

```bash
npm run dev
```

Navigate to ([localhost:5173](http://localhost:5173)) in your browser. You should see a response similar to the following, indicating a successful connection to your Neon database:

```text
PostgreSQL 17.5 (6bc9ef8) on aarch64-unknown-linux-gnu, compiled by gcc (Debian 12.2.0-14+deb12u1) 12.2.0, 64-bit
```

> The specific version may vary depending on the PostgreSQL version of your Neon project.

## Resources

- [RedwoodSDK Documentation](https://docs.rwsdk.com/)
- [Connect to a PostgreSQL database with Cloudflare Workers](https://developers.cloudflare.com/workers/tutorials/postgres/)

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
- [Sveltekit](https://neon.com/docs/guides/sveltekit)
- [Symfony](https://neon.com/docs/guides/symfony)
- [Hono](https://neon.com/docs/guides/hono)
- [Vue](https://neon.com/docs/guides/vue)
