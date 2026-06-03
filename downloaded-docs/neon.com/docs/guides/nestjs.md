> This page location: Frontend & Frameworks > Frameworks > NestJS
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Step-by-step guide for connecting a NestJS application to a Neon project, including project setup, dependency installation, and configuration of database credentials for secure access.

# Connect a NestJS application to Neon

Set up a Neon project in seconds and connect from a NestJS application

Pre-built prompt for connecting NestJS applications to Neon Postgres [View prompt](https://neon.com/prompts/nestjs-prompt.md)

NestJS is a framework for building efficient, scalable Node.js server-side applications[1](https://docs.nestjs.com/). This guide explains how to connect NestJS with Neon using a secure server-side request.

To create a Neon project and access it from a NestJS application:

## Create a Neon project

If you do not have one already, create a Neon project. Save your connection details including your password. They are required when defining connection settings.

1. Navigate to the [Projects](https://console.neon.tech/app/projects) page in the Neon Console.
2. Click **New Project**.
3. Specify your project settings and click **Create Project**.

## Create a NestJS project and add dependencies

1. Create a NestJS project if you do not have one. For instructions, see [Quick Start](https://docs.nestjs.com/first-steps), in the NestJS documentation.

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

Add a `.env` file to your project directory and add your Neon connection string to it. You can find your connection details by clicking **Connect** on the Neon **Project Dashboard**. For more information, see [Connect from any application](https://neon.com/docs/connect/connect-from-any-app).

```shell
DATABASE_URL="postgresql://<user>:<password>@<endpoint_hostname>.neon.tech:<port>/<dbname>?sslmode=require&channel_binding=require"
```

## Configure the Postgres client

### 1. Create a Database Module

To manage the connection to your Neon database, start by creating a **DatabaseModule** in your NestJS application. This module will handle the configuration and provisioning of the Postgres client.

**node-postgres**

```typescript
import { config } from 'dotenv';
import { Module } from '@nestjs/common';
import pg from 'pg';

// Load Environment Variables
config({
  path: ['.env', '.env.production', '.env.local'],
});

const sql = new pg.Pool({ connectionString: process.env.DATABASE_URL });

const dbProvider = {
  provide: 'POSTGRES_POOL',
  useValue: sql,
};

@Module({
  providers: [dbProvider],
  exports: [dbProvider],
})
export class DatabaseModule {}
```

**postgres.js**

```typescript
import { config } from 'dotenv';
import { Module } from '@nestjs/common';
import postgres from 'postgres';

// Load Environment Variables
config({
  path: ['.env', '.env.production', '.env.local'],
});

const sql = postgres(process.env.DATABASE_URL, { ssl: 'require' });

const dbProvider = {
  provide: 'POSTGRES_POOL',
  useValue: sql,
};

@Module({
  providers: [dbProvider],
  exports: [dbProvider],
})
export class DatabaseModule {}
```

**Neon serverless driver**

```typescript
import { config } from 'dotenv';
import { Module } from '@nestjs/common';
import { neon } from '@neondatabase/serverless';

// Load Environment Variables
config({
  path: ['.env', '.env.production', '.env.local'],
});

const sql = neon(process.env.DATABASE_URL);

const dbProvider = {
  provide: 'POSTGRES_POOL',
  useValue: sql,
};

@Module({
  providers: [dbProvider],
  exports: [dbProvider],
})
export class DatabaseModule {}
```

### 2. Create a Service for Database Interaction

Next, implement a service to handle interaction with your Postgres database. This service will use the database connection defined in the DatabaseModule.

**node-postgres**

```typescript
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class AppService {
  constructor(@Inject('POSTGRES_POOL') private readonly sql: any) {}

  async getTable(name: string): Promise<any[]> {
    const client = await this.sql.connect();
    const { rows } = await client.query(`SELECT * FROM ${name}`);
    return rows;
  }
}
```

**postgres.js**

```typescript
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class AppService {
  constructor(@Inject('POSTGRES_POOL') private readonly sql: any) {}

  async getTable(name: string): Promise<any[]> {
    return await this.sql(`SELECT * FROM ${name}`);
  }
}
```

**Neon serverless driver**

```typescript
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class AppService {
  constructor(@Inject('POSTGRES_POOL') private readonly sql: any) {}

  async getTable(name: string): Promise<any[]> {
    return await this.sql(`SELECT * FROM ${name}`);
  }
}
```

### 3. Integrate the Database Module and Service

Import and inject the DatabaseModule and AppService into your AppModule. This ensures that the database connection and services are available throughout your application.

```typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { DatabaseModule } from './database/database.module';

@Module({
  imports: [DatabaseModule],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

### 4. Define a Controller Endpoint

Finally, define a `GET` endpoint in your AppController to fetch data from your Postgres database. This endpoint will use the AppService to query the database.

```typescript
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller('/')
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  async getTable() {
    return this.appService.getTable('playing_with_neon');
  }
}
```

## Run the app

When you run `npm run start` you can expect to see output similar to the following at [localhost:3000](https://neon.com/docs/guides/localhost:3000):

```shell
[{"id":1,"name":"c4ca4238a0","value":0.39330545},{"id":2,"name":"c81e728d9d","value":0.14468245}]
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
