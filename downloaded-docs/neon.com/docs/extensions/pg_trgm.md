> This page location: Extensions > pg_trgm
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup and usage of the `pg_trgm` extension in Postgres for enhancing text search capabilities through trigram matching, enabling efficient fuzzy string matching and proximity searches.

# The pg_trgm extension

Improve Postgres text searches with the pg_trgm extension

The `pg_trgm` extension enhances Postgres' ability to perform text searches by using trigram matching. Trigrams are groups of three consecutive characters taken from a string. By breaking down text into trigrams, Postgres can perform more efficient and flexible searches, such as similarity and proximity searches.

It's especially useful for fuzzy string matching and searching large bodies of text.

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

This guide covers how to enable `pg_trgm`, use it for text searches, and optimize queries.

**Note:** The `pg_trgm` extension is open-source and can be installed on any Postgres setup. Detailed information about the extension is available in the [PostgreSQL Documentation](https://www.postgresql.org/docs/current/pgtrgm.html).

**Version availability**

Please refer to the [list of all extensions](https://neon.com/docs/extensions/pg-extensions) available in Neon for up-to-date information.

## Enable the `pg_trgm` extension

Activate `pg_trgm` by running the `CREATE EXTENSION` statement in your Postgres client:

```sql
CREATE EXTENSION IF NOT EXISTS pg_trgm;
```

For information about using the Neon SQL Editor, see [Query with Neon's SQL Editor](https://neon.com/docs/get-started/query-with-neon-sql-editor). For information about using the `psql` client with Neon, see [Connect with psql](https://neon.com/docs/connect/query-with-psql-editor).

## Example usage

Let's say you're developing a database of books and you want to find books with similar titles. We first create a test table and insert some sample data, using the query below.

```sql
CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title TEXT
);

INSERT INTO books (title)
VALUES
    ('The Great Gatsby'),
    ('The Grapes of Wrath'),
    ('Great Expectations'),
    ('War and Peace'),
    ('Pride and Prejudice'),
    ('To Kill a Mockingbird'),
    ('1984');
```

**Basic string matching**

The `pg_trgm` extension can help you do fuzzy matches on strings.

For example, the query below looks for titles that are similar to the misspelled phrase "Grate Expectation". The `%` operator, provided by `pg_trgm`, measures similarity between two strings based on trigrams, and returns results if the similarity is above a certain threshold.

```sql
SELECT *
FROM books
WHERE title % 'Grate Expectation';
```

This query returns the following:

```text
| id | title               |
|----|---------------------|
| 1  | Great Expectations  |
```

The similarity threshold can be adjusted by setting the `pg_trgm.similarity_threshold` parameter (default value is `0.3`).

## Trigrams

### Counting trigrams

The `pg_trgm` module makes these assumptions about how to count trigrams in a text string:

- Only alphanumeric characters are considered.
- The string is lowercased before counting trigrams.
- Each word is assumed to be prefixed with two spaces and suffixed with one space.
- The set of trigrams output is deduplicated.

We can use the `show_trgm` function to see how `pg_trgm` counts trigrams in a string. Here is an example:

```sql
SELECT show_trgm('War and Peace'); -- {" a"," p"," w"," an"," pe"," wa",ace,and,"ar ","ce ",eac,"nd ",pea,war}
```

### Computing similarity

Given the set of trigrams for two strings `A` and `B`, `pg_trgm` computes the similarity score as the size of the intersection of the two sets divided by the size of the union of the two sets.

Here is an example.

```sql
SELECT show_trgm('War'), show_trgm('Bar'), similarity('War', 'Bar');
```

This query returns the following:

```text
| show_trgm              | show_trgm              | similarity |
|------------------------|------------------------|------------|
| {" w"," wa","ar ",war} | {" b"," ba","ar ",bar} | 0.14285715 |
```

There are 7 distinct trigrams across the two input strings and 1 trigram in common. So the similarity score comes out to be 1/7 (0.14285715).

## Advanced text searching

`pg_trgm` offers powerful tools for more complex text search requirements.

**Proximity search**

The `similarity` function provided by `pg_trgm`, returns a number between 0 and 1, representing how similar the two strings are. By filtering on the similarity score, you can search for strings that are within the specified threshold.

```sql
SELECT title
FROM books
WHERE SIMILARITY(title, 'War and') > 0.3;
```

This query returns the following:

```text
| title         |
|---------------|
| War and Peace |
```

**Substring matching**

`pg_trgm` also provides functionality to match the input text value against substrings within the target string. The query below illustrates this:

```sql
SELECT
    word_similarity('apple', 'green apples'),
    strict_word_similarity('apple', 'green apples');
```

This query returns the following:

```text
| word_similarity | strict_word_similarity |
|-----------------|------------------------|
| 0.8333333       | 0.625                  |
```

The `word_similarity` function returns the maximum similarity score between the input string and any substring of the target string. The similarity score is still computed using trigrams. In this example, the first string `apple` matches with the substring `apple` in the target.

In contrast, the `strict_word_similarity` function only considers a subset of substrings from the target, namely only sequences of full words in the target string. That is, the first string `apple` matches the substring `apples` in the target, hence the lower score.

**Distance scores**

There are operators to calculate the `distance` between two strings, i.e., one minus the similarity score.

```sql
SELECT similarity('Hello', 'Halo') AS similarity, 'Hello' <-> 'Halo' AS distance;
```

This query returns the following:

```text
| similarity | distance  |
|------------|-----------|
| 0.22222222 | 0.7777778 |
```

Similarly, there are operators to compute the distance based on the `word_similarity` and `strict_word_similarity` functions.

## Performance considerations

While `pg_trgm` enhances text search capabilities, computing similarity can get expensive when matching against a large set of strings. Here are a couple of tips to improve performance:

- **Indexing**: Using `pg_trgm`, you can create a `GiST` or `GIN` index to speed up similarity search queries. This also helps regular expression-based searches, such as with `LIKE` and `ILIKE` operators.

  ```sql
  CREATE INDEX trgm_idx_gist ON books USING GIST (title gist_trgm_ops);
  -- or
  CREATE INDEX trgm_idx_gin ON books USING GIN (title gin_trgm_ops);
  ```

- **Limiting results**: Use `LIMIT` to restrict the number of rows returned for more efficient querying.

## Conclusion

`pg_trgm` offers a versatile set of tools for text processing and searching in Postgres. We went over the basics of the extension, including how to enable it and how to use it for fuzzy string matching and proximity searches.

## Resources

- [PostgreSQL pg_trgm documentation](https://www.postgresql.org/docs/current/pgtrgm.html)
- [PostgreSQL Text Search](https://www.postgresql.org/docs/current/textsearch.html)

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
