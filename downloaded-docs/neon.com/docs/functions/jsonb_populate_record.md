> This page location: Postgres guides > Functions > JSON functions > jsonb_populate_record
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the usage of the `jsonb_populate_record` function to populate a record type from a `JSONB` object, facilitating the integration of external JSONB data into existing database records.

# Postgres jsonb_populate_record() function

Casts a JSONB object to a record

The `jsonb_populate_record` function is used to populate a record type with values from a `JSONB` object. It is useful for parsing `JSONB` data received from external sources, particularly when merging it into an existing record.

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

## Function signature

```sql
jsonb_populate_record(base_record ANYELEMENT, json JSONB) -> ANYELEMENT
```

This function takes two arguments: a base record of a row type (which can even be a `NULL` record) and a `JSONB` object. It returns the record updated with the `JSONB` values.

## Example usage

Consider a database table that tracks employee information. When you receive employee information as `JSONB` records, you can use `jsonb_populate_record` to ingest the data into the table.

Here we create the `employees` table with some sample data.

```sql
CREATE TABLE employees (
    id INT,
    name TEXT,
    department TEXT,
    salary NUMERIC
);
```

To illustrate, we start with a `NULL` record and cast the input `JSONB` payload to the `employees` record type.

```sql
INSERT INTO employees
SELECT *
FROM jsonb_populate_record(
    NULL::employees,
    '{"id": "123", "name": "John Doe", "department": "Engineering", "salary": "75000"}'
)
RETURNING *;
```

This query returns the following result:

```text
| id | name     | department  | salary |
|----|----------|-------------|--------|
| 123| John Doe | Engineering | 75000  |
```

## Advanced examples

### Handling partial data with `jsonb_populate_record`

For data points where the `JSONB` objects have missing keys, `jsonb_populate_record` can still cast them into legible records.

Say we receive records for a bunch of employees who are known to be in Sales, but the `department` field is missing from the `JSONB` payload. We can use `jsonb_populate_record` with the default value specified for a field while the other fields are populated from the `JSONB` payload, as in this example:

```sql
INSERT INTO employees
SELECT *
FROM jsonb_populate_record(
    (1, 'ABC', 'Sales', 0)::employees,
    '{"id": "124", "name": "Jane Smith", "salary": "68000"}'
)
RETURNING *;
```

This query returns the following:

```text
| id | name       | department | salary |
|----|------------|------------|--------|
| 124| Jane Smith | Sales      | 68000  |
```

### Using `jsonb_populate_record` with custom types

The base record doesn't need to have the type of a table row and can be a [custom Postgres type](https://www.postgresql.org/docs/current/sql-createtype.html) too. For example, here we first define a custom type `address` and use `jsonb_populate_record` to cast a `JSONB` object to it:

```sql
CREATE TYPE address AS (
    street TEXT,
    city TEXT,
    zip TEXT
);

SELECT *
FROM jsonb_populate_record(
    NULL::address,
    '{"street": "123 Main St", "city": "San Francisco", "zip": "94105"}'
);
```

This query returns the following result:

```text
| street     | city          | zip   |
|------------|---------------|-------|
| 123 Main St| San Francisco | 94105 |
```

## Additional considerations

### Alternative options

- [jsonb_to_record](https://neon.com/docs/functions/jsonb_to_record) - It can be used similarly, with a couple differences. `jsonb_populate_record` can be used with a base record of a pre-defined type, whereas `jsonb_to_record` needs the record type defined inline in the `AS` clause. Further, `jsonb_populate_record` can specify default values for missing fields through the base record, whereas `jsonb_to_record` must assign them NULL values.
- `jsonb_populate_recordset` - It can be used similarly to parse `JSONB`, the difference being that it returns a set of records instead of a single record. For example, if you have an array of `JSONB` objects, you can use `jsonb_populate_recordset` to convert each object into a new row.
- [json_populate_record](https://neon.com/docs/functions/json_populate_record) - It has the same functionality to `jsonb_populate_record`, but accepts `JSON` input instead of `JSONB`.

## Resources

- [Postgres documentation: JSON functions](https://www.postgresql.org/docs/current/functions-json.html)

---

## Related docs (JSON functions)

- [array_to_json](https://neon.com/docs/functions/array_to_json)
- [json](https://neon.com/docs/functions/json)
- [json_agg](https://neon.com/docs/functions/json_agg)
- [json_array_elements](https://neon.com/docs/functions/json_array_elements)
- [json_build_object](https://neon.com/docs/functions/json_build_object)
- [json_each](https://neon.com/docs/functions/json_each)
- [json_exists](https://neon.com/docs/functions/json_exists)
- [json_extract_path](https://neon.com/docs/functions/json_extract_path)
- [json_extract_path_text](https://neon.com/docs/functions/json_extract_path_text)
- [json_object](https://neon.com/docs/functions/json_object)
- [json_populate_record](https://neon.com/docs/functions/json_populate_record)
- [json_query](https://neon.com/docs/functions/json_query)
- [json_scalar](https://neon.com/docs/functions/json_scalar)
- [json_serialize](https://neon.com/docs/functions/json_serialize)
- [json_table](https://neon.com/docs/functions/json_table)
- [json_to_record](https://neon.com/docs/functions/json_to_record)
- [json_value](https://neon.com/docs/functions/json_value)
- [jsonb_array_elements](https://neon.com/docs/functions/jsonb_array_elements)
- [jsonb_each](https://neon.com/docs/functions/jsonb_each)
- [jsonb_extract_path](https://neon.com/docs/functions/jsonb_extract_path)
- [jsonb_extract_path_text](https://neon.com/docs/functions/jsonb_extract_path_text)
- [jsonb_object](https://neon.com/docs/functions/jsonb_object)
- [jsonb_to_record](https://neon.com/docs/functions/jsonb_to_record)
