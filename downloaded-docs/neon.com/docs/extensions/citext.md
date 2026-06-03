> This page location: Extensions > citext
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup and usage of the `citext` extension in Postgres for handling case-insensitive text data, including installation steps and practical examples for applications like user registration systems.

# The citext Extension

Use the citext extension to handle case-insensitive data in Postgres

The `citext` extension in Postgres provides a case-insensitive data type for text. Use it for columns where case shouldn't matter, like usernames or email addresses.

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

This guide covers the `citext` extension: its setup, usage, and practical examples in Postgres. For datasets where consistent text formatting isn't guaranteed, case-insensitive queries can streamline operations.

**Note:** The `citext` extension is an open-source module for Postgres. It can be easily installed and used in any Postgres database. This guide provides steps for installation and usage, with further details available in the [Postgres Documentation](https://postgresql.org/docs/current/citext.html).

## Enable the `citext` extension

You can enable `citext` by running the following `CREATE EXTENSION` statement in the Neon **SQL Editor** or from a client such as `psql` that is connected to Neon.

```sql
CREATE EXTENSION IF NOT EXISTS citext;
```

For information about using the Neon SQL Editor, see [Query with Neon's SQL Editor](https://neon.com/docs/get-started/query-with-neon-sql-editor). For information about using the `psql` client with Neon, see [Connect with psql](https://neon.com/docs/connect/query-with-psql-editor).

## Example usage

**Creating a table with citext**

Consider a user registration system where the user's email should be unique, regardless of case.

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(255) UNIQUE,
    email CITEXT UNIQUE
);
```

In this table, the `email` field is of type `citext`, ensuring that email addresses are treated case-insensitively.

**Inserting data**

Insert data as you would normally. The `citext` type automatically handles case-insensitivity.

```sql
INSERT INTO users (username, email)
VALUES
  ('johnsmith', 'JohnSmith@email.com'),
  ('AliceSmith', 'ALICE@example.com'),
  ('BobJohnson', 'Bob@example.com'),
  ('EveAnderson', 'eve@example.com');
```

**Case-insensitive querying**

Queries against `citext` columns are inherently case-insensitive. Effectively, it calls the `lower()` function on both strings when comparing two values.

```sql
SELECT * FROM users WHERE email = 'johnsmith@email.com';
```

This query returns the following:

```text
| id | username   | email                  |
|----|------------|------------------------|
| 1  | johnsmith  | JohnSmith@email.com    |
```

The email address matched even though the case was different.

## More examples

**Using citext with regex functions**

The `citext` extension can be used with regular expressions and other string-matching functions, which perform string matching in a case-insensitive manner.

For example, the query below finds users whose email addresses start with 'AL'.

```sql
SELECT * FROM users WHERE regexp_match(email, '^AL', 'i') IS NOT NULL;
```

This query returns the following:

```text
| id | username    | email              |
|----|-------------|--------------------|
| 1  | AliceSmith  | ALICE@example.com  |
```

**Using citext data as TEXT**

If you do want case-sensitive behavior, you can cast `citext` data to `text` and use it as shown here:

Query:

```sql
SELECT * FROM users WHERE email::text LIKE '%EVE%';
```

This query will only return results if it finds a user with an email address containing 'EVE'.

## Benefits of Using citext

- **Query simplicity**: No need for functions like `lower()` or `upper()` to perform case-insensitive comparisons.
- **Data integrity**: Helps maintain data consistency, especially in user input scenarios.

## Performance considerations

### Indexing with citext

Indexing `citext` fields is similar to indexing regular text fields. However, it's important to note that the index will be case-insensitive.

```sql
CREATE INDEX idx_email ON users USING btree(email);
```

This index will improve the performance of queries involving the `email` field. Depending on whether the more frequent use case is case-sensitive or case-insensitive, you can choose to index the `citext` field or cast it to `text` and index that.

### Comparison with `lower()` function

`Citext` internally does an operation similar to `lower()` on both sides of the comparison, so there is not a big performance jump. However, using `citext` ensures consistent case-insensitive behavior across queries without the need for repeatedly applying the `lower()` function, which makes errors less likely.

## Conclusion

The `citext` extension helps manage case-insensitivity in text data within Postgres. It simplifies queries and ensures consistency in data handling. This guide provides an overview of using `citext`, including creating and querying case-insensitive fields.

## Resources

- [PostgreSQL citext documentation](https://www.postgresql.org/docs/current/citext.html)

---

## Related docs (Extensions)

- [Extension explorer](https://neon.com/docs/extensions/extension-explorer)
- [anon](https://neon.com/docs/extensions/postgresql-anonymizer)
- [btree_gin](https://neon.com/docs/extensions/btree_gin)
- [btree_gist](https://neon.com/docs/extensions/btree_gist)
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
- [pg_session_jwt](https://neon.com/docs/extensions/pg_session_jwt)
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
