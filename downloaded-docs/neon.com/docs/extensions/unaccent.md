> This page location: Extensions > unaccent
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup and usage of the `unaccent` extension in Postgres for removing accents and diacritics from text, enhancing search capabilities in multilingual applications on the Neon platform.

# The unaccent extension

Remove accents and diacritics for effective text searching in Postgres

The `unaccent` extension for Postgres enables handling of text data in a more user-friendly and language-tolerant way. It allows you to remove [accents/stress](https://en.wikipedia.org/wiki/Stress_\(linguistics\)) ([diacritic signs](https://en.wikipedia.org/wiki/Diacritic)) from text strings, making it easier to perform searches and comparisons that are insensitive to accents. Use it in multilingual applications where users might not consistently use accents when typing search queries.

Imagine a user searching for "Hôtel" but only typing "Hotel". Without `unaccent`, the database might not find the intended results. With `unaccent`, you can ensure that searches are more forgiving and return relevant results regardless of accent variations.

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

This guide will walk you through the essentials of using the `unaccent` extension. You'll learn how to enable the extension on Neon, understand its key concepts, use it in queries, optimize performance with indexing, and consider its limitations.

## Enable the `unaccent` extension

You can enable the extension by running the following `CREATE EXTENSION` statement in the [Neon SQL Editor](https://neon.com/docs/get-started/query-with-neon-sql-editor) or from a client such as [psql](https://neon.com/docs/connect/query-with-psql-editor) that is connected to your Neon database.

```sql
CREATE EXTENSION IF NOT EXISTS unaccent;
```

**Version availability:**

Please refer to the [list of all extensions](https://neon.com/docs/extensions/pg-extensions) available in Neon for up-to-date extension version information.

## Removing accents with `unaccent()`

The primary function provided by the `unaccent` extension is `unaccent()`. This function takes a text input and returns the same text with accents removed.

Let's see it in action with a few examples:

```sql
SELECT unaccent('Hôtel');
-- Hotel

SELECT unaccent('cliché');
-- cliche

SELECT unaccent('naïve');
-- naive

SELECT unaccent('café');
-- cafe

SELECT unaccent('Déjà vu');
-- Deja vu
```

As you can see, `unaccent()` effectively strips the diacritics, transforming words with accents into their unaccented counterparts. This transformation is based on a set of rules that are configurable, allowing for customization to suit specific language needs.

## Practical usage examples

`unaccent` is most commonly used to enhance text searching, making it more forgiving and user-friendly. Let's explore some typical use cases:

### Basic accent-insensitive searching

Imagine you have a product catalog and want users to be able to search for products regardless of whether they use accents or not. For instance, a user might search for "cafe" or "café" and expect to find products containing "café".

Consider a `products` table with the following data:

```sql
CREATE TABLE products (
  id SERIAL PRIMARY KEY,
  name TEXT
);

INSERT INTO products (name) VALUES
  ('cafe'),
  ('café'),
  ('Café'),
  ('Café au lait');
```

Without `unaccent`, a simple `WHERE` clause would differentiate between accented and unaccented characters:

```sql
SELECT * FROM products WHERE name = 'café';
```

```
id  | name
----|------
 2  | café
```

```sql
SELECT * FROM products WHERE name = 'cafe';
```

```
id  | name
----|------
 1  | cafe
```

By applying `unaccent()` to both the column and the search term, you can achieve accent-insensitive matching:

```sql
SELECT * FROM products WHERE unaccent(name) = unaccent('cafe');
```

```
id | name
---|------
 1 | cafe
 2 | café
```

### Case-insensitive and accent-insensitive searching with `ILIKE`

For even more flexible searching, you can combine `unaccent()` with the [`ILIKE`](https://neon.com/postgresql/postgresql-tutorial/postgresql-like#postgresql-extensions-of-the-like-operator) operator for case-insensitive and accent-insensitive searches. This comes in handy for free-text search scenarios.

```sql
SELECT * FROM product WHERE unaccent(name) ILIKE unaccent('%cafe%');
```

```
id | name
---+------
 1 | cafe
 2 | café
 3 | Café
 4 | Café au lait
```

In this example, `ILIKE` handles case-insensitivity (matching 'cafe', 'Cafe', etc.), and `unaccent()` ensures that accents are ignored during the comparison. Applying `unaccent()` to both sides of the `ILIKE` condition is crucial for this to work effectively.

### Integration with Full-Text search

While `unaccent()` can be used directly in `WHERE` clauses, its true power for search applications is realized when integrated into with Postgres full-text search capabilities. `unaccent` is designed as a text search dictionary. By incorporating it into your text search configurations, you can ensure that indexing and searching operations automatically handle accent removal.

This involves creating or modifying text search configurations to include the `unaccent` dictionary in the analysis process. When text is indexed and queried using such a configuration, accents are automatically stripped, leading to efficient and accent-insensitive full-text searches.

**Note: Configuration Modifications in Neon**

It's important to note that because `unaccent` is managed by Neon, modifying the default `unaccent.rules` file or other configuration settings requires administrative privileges that are not available to Neon users.  If you have specific needs for customized `unaccent` rules or configurations, please [open a support ticket](https://console.neon.tech/app/projects?modal=support) to discuss your requirements with Neon support.

## Performance and Indexing Considerations

Using `unaccent()` in queries can have performance implications, especially in large tables. Applying functions in `WHERE` clauses often prevents the database from efficiently using standard indexes.

### Indexing with `unaccent()`

Directly indexing an expression like `unaccent(column)` typically doesn't work efficiently because, by default, `unaccent()` is not marked as an `IMMUTABLE` function. Postgres requires functions used in index expressions to be `IMMUTABLE` to guarantee consistent index usage.

To enable indexing with `unaccent()`, you can create an `IMMUTABLE` wrapper function around it. This wrapper function essentially tells Postgres that the function's output will always be the same for a given input, allowing it to be used in index expressions.

Here's an example of creating an `IMMUTABLE` wrapper function:

```sql
CREATE OR REPLACE FUNCTION f_unaccent(text) RETURNS text
AS $$
  SELECT public.unaccent('public.unaccent', $1);
$$ LANGUAGE sql IMMUTABLE PARALLEL SAFE STRICT;
```

**Explanation:**

- `CREATE OR REPLACE FUNCTION f_unaccent(text) RETURNS text`: This defines a new function named `f_unaccent` that takes text as input and returns text.
- `AS $$ ... $$ LANGUAGE sql IMMUTABLE PARALLEL SAFE STRICT`: This is the function body, written in SQL, and the important part is declaring it `IMMUTABLE`.
- `SELECT public.unaccent('public.unaccent', $1);`: Inside the wrapper, we are calling the original `unaccent` function, making sure to schema-qualify it as `public.unaccent` for robustness.

Once you have this `IMMUTABLE` wrapper function, you can create indexes on it:

```sql
CREATE INDEX idx_products_name_unaccent ON products (f_unaccent(name));
```

Now, queries using `f_unaccent(name)` in the `WHERE` clause can use this index, significantly improving performance for accent-insensitive searches.

```sql
SELECT * FROM products WHERE f_unaccent(name) = f_unaccent('cafe');
-- This query can now use the 'idx_products_name_unaccent' index
```

**Alternative: Generated columns**

Another strategy for optimizing performance is to use generated columns. You can add a new column to your table that stores the unaccented version of your text data. This column can then be indexed directly and queried efficiently.

```sql
ALTER TABLE products ADD COLUMN name_unaccented text
GENERATED ALWAYS AS (f_unaccent(name)) STORED;

CREATE INDEX idx_products_name_unaccent_generated ON products (name_unaccented);

SELECT * FROM products WHERE name_unaccented = 'cafe';
-- This query will use the 'idx_products_name_unaccent_generated' index to find rows where unaccented 'name' matches 'cafe'
```

Generated columns add storage overhead but can offer performance benefits for read-heavy workloads.

## Limitations

While `unaccent` is very useful, it's important to be aware of its limitations:

- **Rule-based:** `unaccent` operates based on a predefined set of rules defined in its configuration file (`unaccent.rules`). The effectiveness of accent removal depends on the completeness and accuracy of these rules for your target languages.
- **Language specificity:** The default rules are primarily geared towards European languages. For languages with different diacritic systems or complex character transformations, the default rules might not be sufficient, and customization of the `unaccent.rules` file might be required.
- **No contextual understanding:** `unaccent` performs a character-by-character transformation based on its rules. It does not understand the context or meaning of words. In some cases, this might lead to over-simplification or loss of subtle distinctions in meaning that accents might convey in certain languages.

## Conclusion

The `unaccent` extension is a valuable tool for handling text data in Postgres, especially in multilingual applications where accent-insensitive searching is essential. By enabling `unaccent`, you can ensure that your database is more user-friendly and tolerant of accent variations in search queries.

## Resources

- [`unaccent` Extension in PostgreSQL Documentation](https://www.postgresql.org/docs/current/unaccent.html)

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
- [uuid-ossp](https://neon.com/docs/extensions/uuid-ossp)
- [wal2json](https://neon.com/docs/extensions/wal2json)
- [xml2](https://neon.com/docs/extensions/xml2)
