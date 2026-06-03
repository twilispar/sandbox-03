> This page location: Extensions > xml2
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup and functionality of the `xml2` extension in Postgres, enabling XML parsing, XPath querying, and XSLT transformations for XML data management within Neon databases.

# The xml2 extension

Perform XPath querying and XSLT transformations on XML data in Postgres.

The `xml2` extension for Postgres provides functions to parse XML data, evaluate XPath queries against it, and perform XSLT transformations. This can be useful for applications that need to process or extract information from XML documents stored within the database.

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

## Enable the `xml2` extension

You can enable the extension by running the following `CREATE EXTENSION` statement in the [Neon SQL Editor](https://neon.com/docs/get-started/query-with-neon-sql-editor) or from a client such as [psql](https://neon.com/docs/connect/query-with-psql-editor) that is connected to your Neon database.

```sql
CREATE EXTENSION IF NOT EXISTS xml2;
```

**Version availability:**

Please refer to the [list of all extensions](https://neon.com/docs/extensions/pg-extensions) available in Neon for up-to-date extension version information.

**Note:** The `xml2` extension was developed to provide robust XML processing capabilities within Postgres before the SQL/XML standard features were fully integrated. While it offers useful functions for XPath querying and XSLT, the SQL/XML standard now provides a more comprehensive and standardized approach to XML manipulation.

## `xml2` functions

The `xml2` module provides functions for XML parsing, XPath querying, and XSLT transformations.

### XML parsing and validation

- **`xml_valid(document text) → boolean`**
  Parses the given XML document string and returns `true` if it is well-formed XML, `false` otherwise.

  ```sql
  SELECT xml_valid('<book><title>My Book</title></book>');
  -- true

  SELECT xml_valid('<book><title>My Book</title>');
  -- false (not well-formed)
  ```

### XPath querying functions

These functions evaluate an XPath expression on a given XML document.

- **`xpath_string(document text, query text) → text`**
  Evaluates the XPath query and casts the result to a text string.

  ```sql
  SELECT xpath_string('<book><title>My Adventures</title></book>', '/book/title/text()');
  -- My Adventures
  ```

- **`xpath_number(document text, query text) → real`**
  Evaluates the XPath query and casts the result to a real number.

  ```sql
  SELECT xpath_number('<book><price>19.95</price></book>', '/book/price/text()');
  -- 19.95
  ```

- **`xpath_bool(document text, query text) → boolean`**
  Evaluates the XPath query and casts the result to a boolean.

  ```sql
  SELECT xpath_bool('<book available="true"></book>', '/book/@available="true"');
  -- true
  ```

- **`xpath_nodeset(document text, query text, toptag text, itemtag text) → text`**
  Evaluates the query and wraps the resulting nodeset in the specified `toptag` and `itemtag` XML tags. If `toptag` or `itemtag` is an empty string, the respective tag is omitted.
  There are also two-argument and three-argument versions:

  - `xpath_nodeset(document text, query text)`: Omits both `toptag` and `itemtag`.
  - `xpath_nodeset(document text, query text, itemtag text)`: Omits `toptag`.

  ```sql
  SELECT xpath_nodeset(
      '<books><book><title>Book A</title></book><book><title>Book B</title></book></books>',
      '//title',
      'results',
      'entry'
  );
  -- <results><entry><title>Book A</title></entry><entry><title>Book B</title></entry></results>

  SELECT xpath_nodeset(
      '<books><book><title>Book A</title></book></books>',
      '//title/text()'
  );
  -- Book A

  -- To get XML nodes:
  SELECT xpath_nodeset(
      '<books><book><title>Book A</title></book><book><title>Book B</title></book></books>',
      '//title'
  );
  -- <title>Book A</title><title>Book B</title>
  ```

- **`xpath_list(document text, query text, separator text) → text`**
  Evaluates the query and returns multiple text values separated by the specified `separator`.
  There is also a two-argument version `xpath_list(document text, query text)` which uses a comma (`,`) as the separator.

  ```sql
  SELECT xpath_list(
      '<books><book><author>Author 1</author><author>Author 2</author></book></books>',
      '//author/text()',
      '; '
  );
  -- Author 1; Author 2
  ```

### `xpath_table` function

The `xpath_table` function is a powerful tool for extracting data from a set of XML documents and returning it as a relational table.

`xpath_table(key text, document text, relation text, xpaths text, criteria text) returns setof record`

**Parameters:**

- `key`: The name of the "key" field from the source table. This field identifies the record from which each output row came and is returned as the first column.
- `document`: The name of the field in the source table containing the XML document.
- `relation`: The name of the table or view containing the XML documents.
- `xpaths`: One or more XPath expressions, separated by `|`, to extract data.
- `criteria`: The content of a `WHERE` clause to filter rows from the `relation`. This cannot be omitted; use `true` to process all rows.

The function constructs and executes a SQL `SELECT` statement internally. The `key` and `document` parameters must resolve to exactly two columns in this internal select.

`xpath_table` must be used in a `FROM` clause, and an `AS` clause is required to define the output column names and types. The first column in the `AS` clause corresponds to the `key`.

**Example:**

Suppose you have a table `catalog_items`:

```sql
CREATE TABLE catalog_items (
    item_sku TEXT PRIMARY KEY,
    item_details XML,
    added_on_date DATE
);

INSERT INTO catalog_items (item_sku, item_details, added_on_date) VALUES
('WDGT-001', XMLPARSE(DOCUMENT '<item><name>Super Widget</name><stock_level>150</stock_level><category>Gadgets</category></item>'), '2025-03-10'),
('TOOL-005', XMLPARSE(DOCUMENT '<item><name>Mega Wrench</name><stock_level>75</stock_level><category>Tools</category></item>'), '2025-04-02');
```

You can use `xpath_table` to extract data:

```sql
SELECT * FROM
    xpath_table(
        'item_sku',         -- The key column from catalog_items
        'item_details',     -- The XML column from catalog_items
        'catalog_items',    -- The source table
        '/item/name/text()|/item/stock_level/text()|/item/category/text()', -- XPath expressions
        'added_on_date >= ''2025-01-01'''  -- Criteria for filtering
    ) AS extracted_data(     -- Alias for the output table and its columns
        product_sku TEXT,
        product_name TEXT,
        current_stock INTEGER,
        product_category TEXT
    );
```

**Output:**

| product\_sku | product\_name | current\_stock | product\_category |
| :----------- | :------------ | :------------- | :---------------- |
| WDGT-001     | Super Widget  | 150            | Gadgets           |
| TOOL-005     | Mega Wrench   | 75             | Tools             |

**Data type conversion:**
`xpath_table` internally deals with string representations of XPath results. When you specify a data type (for example, `INTEGER`) in the `AS` clause, Postgres attempts to convert the string to that type. If conversion fails (for example, an empty string or non-numeric text to `INTEGER`), an error occurs. It might be safer to extract as `TEXT` and then cast explicitly if data quality is uncertain.

### XSLT functions

The `xml2` extension provides functions for XSLT (Extensible Stylesheet Language Transformations).

- **`xslt_process(document text, stylesheet text, paramlist text) returns text`**
  Applies the XSL `stylesheet` to the XML `document` and returns the transformed text. The `paramlist` argument accepts a string containing parameter assignments for the transformation, formatted as key-value pairs separated by commas (for example, `'name=value,debug=1'`). It's important to note that due to the straightforward parsing mechanism, individual parameter values within this list cannot themselves contain commas.

- **`xslt_process(document text, stylesheet text) returns text`**
  A two-parameter version that applies the stylesheet without passing any external parameters.

**Example:**

Let's say you have an XML document `my_data.xml`:

```xml
<data><item>Hello</item></data>
```

And `my_stylesheet.xsl` contains an XSLT to transform `<data><item>Hello</item></data>` into `<message>Hello</message>`:

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:template match="/data/item">
    <message><xsl:value-of select="."/></message>
  </xsl:template>
</xsl:stylesheet>
```

You can apply the XSLT transformation using `xslt_process`. Here's an example of how to do this in Postgres:

```sql
DO $$
DECLARE
  xml_doc TEXT := '<data><item>Hello</item></data>';
  xslt_style TEXT := '<?xml version="1.0"?><xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"><xsl:output omit-xml-declaration="yes"/><xsl:template match="/data/item"><message><xsl:value-of select="."/></message></xsl:template></xsl:stylesheet>';
  transformed_xml TEXT;
BEGIN
  transformed_xml := xslt_process(xml_doc, xslt_style);
  RAISE NOTICE '%', transformed_xml;
END $$;
-- Output: <message>Hello</message>
```

## Conclusion

The `xml2` extension lets you parse, query, and transform XML documents using XPath and XSLT directly within Postgres.

## Resources

- [PostgreSQL `xml2` documentation](https://www.Postgres.org/docs/current/xml2.html)
- [PostgreSQL XML Data Type](https://neon.com/postgresql/postgresql-tutorial/postgresql-xml-data-type)

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
- [unaccent](https://neon.com/docs/extensions/unaccent)
- [uuid-ossp](https://neon.com/docs/extensions/uuid-ossp)
- [wal2json](https://neon.com/docs/extensions/wal2json)
