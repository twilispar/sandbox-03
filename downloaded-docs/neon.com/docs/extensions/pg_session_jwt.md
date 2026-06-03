> This page location: Extensions > pg_session_jwt
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup of the `pg_session_jwt` extension in Postgres for managing authenticated sessions using JSON Web Tokens (JWTs), including JWK validation and PostgREST compatibility for secure user identity handling.

# The pg_session_jwt extension

Handle authenticated sessions through JWTs in Postgres

**Related resources**

- [Neon Data API](https://neon.com/docs/data-api/overview)
- [Custom authentication providers](https://neon.com/docs/data-api/custom-authentication-providers)
- [Row-Level Security (RLS)](https://neon.com/docs/guides/row-level-security)

**Source code**

- [pg_session_jwt on GitHub](https://github.com/neondatabase/pg_session_jwt)

**Important:** The `pg_session_jwt` extension is automatically installed when you enable the [Neon Data API](https://neon.com/docs/data-api/overview) for a branch. Do not install this extension manually.

The `pg_session_jwt` extension is a Postgres extension designed to handle authenticated sessions through JSON Web Tokens (JWTs). When configured with a JWK (JSON Web Key), it verifies JWT authenticity. When operating without a JWK, it falls back to using PostgREST-compatible JWT claims.

This extension powers the [Neon Data API](https://neon.com/docs/data-api/overview), enabling secure session management and Row-Level Security (RLS) based on user identity.

## Features

- **JWT session initialization** using a JWK (JSON Web Key) for secure JWT validation
- **Flexible authentication modes**: use either JWK-validated JWTs or PostgREST-compatible JWT claims
- **User ID retrieval** directly from the database for use in RLS policies
- **JSONB-based storage** and retrieval of session information

## How it works

The extension can operate in two modes:

### With JWK validation

When a JWK is configured, the extension validates JWT signatures and extracts user information from verified tokens. This is the mode used by the Neon Data API.

### With PostgREST-compatible JWT claims

When operating without a JWK, the extension works with PostgREST-compatible JWT claims via the `request.jwt.claims` parameter. This provides compatibility with PostgREST's JWT handling.

## Functions

The `pg_session_jwt` extension provides functions in the `auth` schema:

### auth.user_id()

Returns the user ID (`sub` claim) from the current session's JWT.

```sql
SELECT auth.user_id();
```

This function is commonly used in RLS policies to filter data by the authenticated user:

```sql
CREATE POLICY "Users can only see their own data"
  ON todos
  FOR SELECT
  USING (user_id = auth.user_id());
```

### auth.session()

Returns the entire JWT payload as JSONB, giving you access to all claims in the token.

```sql
SELECT auth.session();
```

### auth.jwt()

Alias for `auth.session()`.

### auth.uid()

Similar to `auth.user_id()` but returns UUID type. Expects the `sub` claim to be a valid UUID, otherwise returns NULL.

```sql
SELECT auth.uid();
```

## Usage with Neon Data API

The `pg_session_jwt` extension is automatically configured when you enable the [Neon Data API](https://neon.com/docs/data-api/overview). The Data API handles JWT validation using your configured authentication provider's JWKS URL.

When making requests to the Data API, include your JWT in the `Authorization` header:

```http
GET https://your-project.data.neon.tech/v1/todos
Authorization: Bearer <your-jwt-token>
```

The Data API validates the token and makes the user identity available via `auth.user_id()` for your RLS policies.

## Usage with custom setups

For custom implementations outside of the Neon Data API, you can configure the extension manually:

### Initialize with JWK

Set the JWK at connection time using libpq options:

```bash
export PGOPTIONS="-c pg_session_jwt.jwk=$MY_JWK"
```

Then in your session:

```sql
-- Initialize the session with the configured JWK
SELECT auth.init();

-- Set the JWT for the current session
SELECT auth.jwt_session_init('your.jwt.token');

-- Now you can use auth functions
SELECT auth.user_id();
```

### Using PostgREST-compatible claims

When no JWK is configured, set claims via the `request.jwt.claims` parameter:

```sql
SET request.jwt.claims = '{"sub": "user-123", "role": "authenticated"}';
SELECT auth.user_id();  -- Returns 'user-123'
```

**Warning:** When using the fallback mode without JWK validation, `request.jwt.claims` is a regular Postgres parameter that can be modified by any database user. Ensure your application sets these claims securely before executing user queries.

## References

- [pg_session_jwt on GitHub](https://github.com/neondatabase/pg_session_jwt)
- [Neon Data API documentation](https://neon.com/docs/data-api/overview)
- [Custom authentication providers](https://neon.com/docs/data-api/custom-authentication-providers)
- [Row-Level Security guide](https://neon.com/docs/guides/row-level-security)

---

## Related docs (Extensions)

- [Extension explorer](https://neon.com/docs/extensions/extension-explorer)
- [anon](https://neon.com/docs/extensions/postgresql-anonymizer)
- [btree_gin](https://neon.com/docs/extensions/btree_gin)
- [btree_gist](https://neon.com/docs/extensions/btree_gist)
- [citext](https://neon.com/docs/extensions/citext)
- [cube](https://neon.com/docs/extensions/cube)
- [dblink](https://neon.com/docs/extensions/dblink)
- [dict_int](https://neon.com/docs/extensions/dict_int)
- [earthdistance](https://neon.com/docs/extensions/earthdistance)
- [fuzzystrmatch](https://neon.com/docs/extensions/fuzzystrmatch)
- [hstore](https://neon.com/docs/extensions/hstore)
- [intarray](https://neon.com/docs/extensions/intarray)
- [ltree](https://neon.com/docs/extensions/ltree)
- [neon](https://neon.com/docs/extensions/neon)
- [neon_utils](https://neon.com/docs/extensions/neon-utils)
- [online_advisor](https://neon.com/docs/extensions/online_advisor)
- [pgcrypto](https://neon.com/docs/extensions/pgcrypto)
- [pgvector](https://neon.com/docs/extensions/pgvector)
- [pgrag](https://neon.com/docs/extensions/pgrag)
- [pg_cron](https://neon.com/docs/extensions/pg_cron)
- [pg_graphql](https://neon.com/docs/extensions/pg_graphql)
- [pg_mooncake](https://neon.com/docs/extensions/pg_mooncake)
- [pg_partman](https://neon.com/docs/extensions/pg_partman)
- [pg_prewarm](https://neon.com/docs/extensions/pg_prewarm)
- [pg_stat_statements](https://neon.com/docs/extensions/pg_stat_statements)
- [pg_repack](https://neon.com/docs/extensions/pg_repack)
- [pg_search](https://neon.com/docs/extensions/pg_search)
- [pg_tiktoken](https://neon.com/docs/extensions/pg_tiktoken)
- [pg_trgm](https://neon.com/docs/extensions/pg_trgm)
- [pg_uuidv7](https://neon.com/docs/extensions/pg_uuidv7)
- [pgrowlocks](https://neon.com/docs/extensions/pgrowlocks)
- [pgstattuple](https://neon.com/docs/extensions/pgstattuple)
- [plv8](https://neon.com/docs/extensions/plv8)
- [postgis](https://neon.com/docs/extensions/postgis)
- [postgis-related](https://neon.com/docs/extensions/postgis-related-extensions)
- [postgres_fdw](https://neon.com/docs/extensions/postgres_fdw)
- [tablefunc](https://neon.com/docs/extensions/tablefunc)
- [timescaledb](https://neon.com/docs/extensions/timescaledb)
- [unaccent](https://neon.com/docs/extensions/unaccent)
- [uuid-ossp](https://neon.com/docs/extensions/uuid-ossp)
- [wal2json](https://neon.com/docs/extensions/wal2json)
- [xml2](https://neon.com/docs/extensions/xml2)
