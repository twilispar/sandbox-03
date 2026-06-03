> This page location: Extensions > btree_gin
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup and functionality of the `btree_gin` extension in Postgres, enabling efficient multicolumn GIN indexing for both complex and simple data types to optimize query performance.

# The btree_gin extension

Combine GIN and B-tree indexing capabilities for efficient multi-column queries in Postgres

The `btree_gin` extension for Postgres provides a specialized set of **GIN operator classes** that allow common, "B-tree-like" data types to be included in **GIN indexes**. Use it when you need **multicolumn GIN indexes** that combine complex data types (like arrays or JSONB) with simpler types such as integers, timestamps, or text. Ultimately, `btree_gin` extends GIN to a broader range of indexing needs, optimizing queries across diverse data structures.

Consider a scenario where an application needs to query blog posts based on a set of `tags` (an array) and a `publication_date` (a timestamp). The `btree_gin` extension allows for a single, optimized index to service both conditions, potentially offering significant performance gains over alternative indexing strategies.

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

## Enable the `btree_gin` extension

You can enable the extension by running the following `CREATE EXTENSION` statement in the [Neon SQL Editor](https://neon.com/docs/get-started/query-with-neon-sql-editor) or from a client such as [psql](https://neon.com/docs/connect/query-with-psql-editor) that is connected to your Neon database.

```sql
CREATE EXTENSION IF NOT EXISTS btree_gin;
```

**Version availability:**

Please refer to the [list of all extensions](https://neon.com/docs/extensions/pg-extensions) available in Neon for up-to-date extension version information.

## `btree_gin`: Bridging index types

A common challenge arises when queries require filtering on both B-tree friendly columns (for example, `status TEXT`, `created_at TIMESTAMP`) and GIN-friendly columns (for example, `attributes JSONB`, `tags TEXT[]`). While Postgres can use separate B-tree and GIN indexes and combine their results, this is not always the most performant approach.

The `btree_gin` extension addresses this by providing GIN **operator classes** for many standard B-tree-indexable data types. These operator classes instruct the GIN indexing mechanism on how to handle these scalar types as if they were native GIN-indexable items.

For instance, with `btree_gin`, a single GIN index can be defined on `(order_date TIMESTAMP, product_tags TEXT[])`.

```sql
-- Create the table
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    order_date TIMESTAMP,
    product_tags TEXT[]
);

CREATE INDEX idx_orders_date_tags
ON orders
USING GIN (order_date, product_tags);
```

Postgres can then use this composite index to optimize queries filtering on both `order_date` and `product_tags` simultaneously, such as:

```sql
SELECT * FROM orders
WHERE order_date >= '2025-04-01' AND order_date < '2025-05-01'
  AND product_tags @> ARRAY['electronics'];
```

Without `btree_gin`, `order_date` could not be directly included in a GIN index in this manner.

## Usage scenarios

Let's explore some practical examples of how `btree_gin` can be applied to real-world scenarios, particularly in the context of filtering and querying data efficiently.

### Filtering posts by tags and publication date

Consider a `posts` table where queries frequently target posts with specific tags published within a defined timeframe.

#### Table schema

```sql
CREATE TABLE posts (
    post_id SERIAL PRIMARY KEY,
    title TEXT,
    content TEXT,
    tags TEXT[],             -- GIN-friendly array
    published_at TIMESTAMPTZ -- B-tree friendly timestamp
);

INSERT INTO posts (title, tags, published_at) VALUES
('Postgres Performance Tuning', '{"postgres", "performance", "database"}', '2025-03-15 10:30:00Z'),
('Advanced Indexing Strategies', '{"sql", "indexes", "optimization"}', '2025-04-02 14:00:00Z'),
('Working with JSONB in Postgres', '{"postgres", "jsonb", "nosql"}', '2025-04-20 09:15:00Z');
```

#### `btree_gin` index creation

A composite GIN index is created to cover both `tags` and `published_at`.

```sql
CREATE INDEX idx_posts_tags_published
ON posts
USING GIN (tags, published_at);
```

#### Example query

Retrieve posts tagged 'postgres' published in April 2025.

```sql
SELECT title, tags, published_at
FROM posts
WHERE tags @> '{"postgres"}'
  AND published_at >= '2025-04-01 00:00:00Z'
  AND published_at < '2025-05-01 00:00:00Z';
```

The `idx_posts_tags_published` index enables Postgres to efficiently process both the array containment (`@>`) and timestamp range conditions.

### E-commerce product filtering by attributes and price

In an e-commerce context, users often filter products based on dynamic attributes (for example, stored in `JSONB`) and price ranges.

#### Table schema

```sql
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name TEXT,
    attributes JSONB,       -- GIN-friendly JSONB (for example, {"color": "red", "material": "cotton"})
    price NUMERIC(10, 2)    -- B-tree friendly numeric
);

INSERT INTO products (name, attributes, price) VALUES
('Men''s Cotton T-Shirt', '{"color": "blue", "size": "M", "material": "cotton"}', 29.99),
('Women''s Wool Sweater', '{"color": "red", "size": "S", "material": "wool"}', 89.50),
('Unisex Denim Jeans', '{"color": "black", "size": "32/30", "material": "denim"}', 59.95);
```

#### `btree_gin` index creation

```sql
CREATE INDEX idx_products_attributes_price
ON products
USING GIN (attributes, price);
```

#### Example query

Find products made of "cotton" with a price below $50.

```sql
SELECT name, attributes, price
FROM products
WHERE attributes @> '{"material": "cotton"}' AND price < 50.00;
```

The `idx_products_attributes_price` index handles both the JSONB containment check and the numeric inequality efficiently.

## Important considerations and Best practices

- **Write performance impact:** GIN indexes, due to their structure, generally incur a higher cost for `INSERT`, `UPDATE`, and `DELETE` operations compared to B-tree indexes. This should be a consideration for write-intensive workloads.
- **Index storage size:** GIN indexes can be larger on disk than their B-tree counterparts for equivalent data.
- **Query selectivity:** The benefits of `btree_gin` are most pronounced when queries filter on multiple columns included in the index, and the combined predicate is reasonably selective.
- **Dedicated B-tree indexes:** For queries filtering _solely_ on a B-tree-indexable column, a dedicated B-tree index on that column typically offers superior performance. `btree_gin` is primarily for _combined_ criteria.

## Conclusion

The `btree_gin` extension provides a valuable mechanism for optimizing complex queries in Postgres that involve filters across both GIN-indexable and B-tree-indexable column types. By enabling the creation of unified multi-column GIN indexes, `btree_gin` can lead to more efficient query plans, reduced execution times, and a simplified indexing landscape for specific workloads.

## Resources

- [PostgreSQL `btree_gin` documentation](https://www.Postgres.org/docs/current/btree-gin.html)
- [PostgreSQL Indexes](https://neon.com/postgresql/postgresql-indexes)
- [PostgreSQL Index Types](https://neon.com/postgresql/postgresql-indexes/postgresql-index-types)

---

## Related docs (Extensions)

- [Extension explorer](https://neon.com/docs/extensions/extension-explorer)
- [anon](https://neon.com/docs/extensions/postgresql-anonymizer)
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
- [unaccent](https://neon.com/docs/extensions/unaccent)
- [uuid-ossp](https://neon.com/docs/extensions/uuid-ossp)
- [wal2json](https://neon.com/docs/extensions/wal2json)
- [xml2](https://neon.com/docs/extensions/xml2)
