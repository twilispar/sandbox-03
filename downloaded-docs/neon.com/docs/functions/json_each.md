> This page location: Postgres guides > Functions > JSON functions > json_each
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the usage of the `json_each` function in Postgres to expand a JSON object into key-value pairs, enabling iteration and data transformation for dynamic JSON structures.

# Postgres json_each() function

Expands JSON into a record per key-value pair

The `json_each` function in Postgres is used to expand a `JSON` object into a set of key-value pairs.

It is useful when you need to iterate over a `JSON` object's keys and values, such as when you're working with dynamic `JSON` structures where the schema is not fixed. Another important use case is performing data transformations and analytics.

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

## Function signature

```sql
json_each(json JSON) -> SETOF record(key text, value json)
```

The function returns a set of rows, each containing a key and the corresponding value for each field in the input `JSON` object. The key is of type `text`, while the value is of type `json`.

## Example usage

Consider a `JSON` object representing a user's profile information. The `JSON` data will have multiple attributes and might look like this:

```json
{
  "username": "johndoe",
  "age": 30,
  "email": "johndoe@example.com"
}
```

We can go over all the fields in the profile `JSON` object using `json_each`, and produce a row for each key-value pair.

```sql
SELECT key, value
FROM json_each('{"username": "johndoe", "age": 30, "email": "johndoe@example.com"}');
```

This query returns the following results:

```text
| key      | value                 |
|----------|-----------------------|
| username | "johndoe"             |
| age      | 30                    |
| email    | "johndoe@example.com" |
```

## Advanced examples

### `json_each` custom column names

You can use `AS` to specify custom column names for the key and value columns.

```sql
SELECT attr_name, attr_value
FROM json_each('{"username": "johndoe", "age": 30, "email": "johndoe@example.com"}')
AS user_data(attr_name, attr_value);
```

This query returns the following results:

```text
| attr_name | attr_value            |
|-----------|-----------------------|
| username  | "johndoe"             |
| age       | 30                    |
| email     | "johndoe@example.com" |
```

### Use `json_each` as a table or row source

Since `json_each` returns a set of rows, you can use it as a table source in a `FROM` clause. This lets us join the expanded `JSON` data in the output with other tables.

Here, we're joining each row in the `user_data` table with the output of `json_each`:

```sql
CREATE TABLE user_data (
    id INT,
    profile JSON
);
INSERT INTO user_data (id, profile)
VALUES
    (123, '{"username": "johndoe", "age": 30, "email": "johndoe@example.com"}'),
    (140, '{"username": "mikesmith", "age": 40, "email": "mikesmith@example.com"}');

SELECT id, key, value
FROM user_data, json_each(user_data.profile);
```

This query returns the following results:

```text
| id  | key      | value                   |
|-----|----------|-------------------------|
| 123 | username | "johndoe"               |
| 123 | age      | 30                      |
| 123 | email    | "johndoe@example.com"   |
| 140 | username | "mikesmith"             |
| 140 | age      | 40                      |
| 140 | email    | "mikesmith@example.com" |
```

## Additional considerations

### Performance implications

When working with large `JSON` objects, `json_each` may lead to performance overhead, as it expands each key-value pair into a separate row.

### Alternative functions

- `json_each_text` - Similar functionality to `json_each` but returns the value as a text type instead of `JSON`.
- `json_object_keys` - It returns only the set of keys in the `JSON` object, without the values.
- `jsonb_each` - It provides the same functionality as `json_each`, but accepts `JSONB` input instead of `JSON`.

## Resources

- [PostgreSQL documentation: JSON functions](https://www.postgresql.org/docs/current/functions-json.html)

---

## Related docs (JSON functions)

- [array_to_json](https://neon.com/docs/functions/array_to_json)
- [json](https://neon.com/docs/functions/json)
- [json_agg](https://neon.com/docs/functions/json_agg)
- [json_array_elements](https://neon.com/docs/functions/json_array_elements)
- [json_build_object](https://neon.com/docs/functions/json_build_object)
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
- [jsonb_populate_record](https://neon.com/docs/functions/jsonb_populate_record)
- [jsonb_to_record](https://neon.com/docs/functions/jsonb_to_record)
