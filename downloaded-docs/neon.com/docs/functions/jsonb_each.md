> This page location: Postgres guides > Functions > JSON functions > jsonb_each
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the usage of the `jsonb_each()` function in Postgres to expand a `JSONB` object into key-value pairs, facilitating data iteration and transformations.

# Postgres jsonb_each() function

Expands JSONB into a record per key-value pair

The `jsonb_each` function in Postgres is used to expand a `JSONB` object into a set of key-value pairs.

It is useful when you need to iterate over a `JSONB` object's keys and values, such as when you're working with dynamic `JSONB` structures where the schema is not fixed. Another important use case is performing data transformations and analytics.

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

## Function signature

```sql
jsonb_each(json JSON) -> SETOF record(key text, value json)
```

The function returns a set of rows, each containing a key and the corresponding value for each field in the input `JSONB` object. The key is of type `text`, while the value is of type `JSONB`.

## Example usage

Consider a `JSONB` object representing a user's profile information. The `JSONB` data will have multiple attributes and might look like this:

```json
{
  "username": "johndoe",
  "age": 30,
  "email": "johndoe@example.com"
}
```

We can go over all the fields in the profile `JSONB` object using `jsonb_each`, and produce a row for each key-value pair.

```sql
SELECT key, value
FROM jsonb_each('{"username": "johndoe", "age": 30, "email": "johndoe@example.com"}');
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

### Assign custom names to columns output by `jsonb_each`

You can use `AS` to specify custom column names for the key and value columns.

```sql
SELECT attr_name, attr_value
FROM jsonb_each('{"username": "johndoe", "age": 30, "email": "johndoe@example.com"}')
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

### Use `jsonb_each` output as a table or row source

Since `jsonb_each` returns a set of rows, you can use it as a table source in a `FROM` clause. This lets us join the expanded `JSONB` data in the output with other tables.

Here, we're joining each row in the `user_data` table with the output of `jsonb_each`:

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
FROM user_data, jsonb_each(user_data.profile);
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

When working with large `JSONB` objects, `jsonb_each` may lead to performance overhead, as it expands each key-value pair into a separate row.

### Alternative functions

- `jsonb_each_text` - Similar functionality to `jsonb_each` but returns the value as a text type instead of `JSONB`.
- `jsonb_object_keys` - It returns only the set of keys in the `JSONB` object, without the values.
- [json_each](https://neon.com/docs/functions/json_each) - It provides the same functionality as `jsonb_each`, but accepts `JSON` input instead of `JSONB`.

## Resources

- [PostgreSQL documentation: JSON functions](https://www.postgresql.org/docs/current/functions-json.html)

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
- [jsonb_extract_path](https://neon.com/docs/functions/jsonb_extract_path)
- [jsonb_extract_path_text](https://neon.com/docs/functions/jsonb_extract_path_text)
- [jsonb_object](https://neon.com/docs/functions/jsonb_object)
- [jsonb_populate_record](https://neon.com/docs/functions/jsonb_populate_record)
- [jsonb_to_record](https://neon.com/docs/functions/jsonb_to_record)
