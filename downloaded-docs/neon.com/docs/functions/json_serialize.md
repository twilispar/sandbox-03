> This page location: Postgres guides > Functions > JSON functions > json_serialize
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the usage of the `json_serialize()` function in PostgreSQL 17 for converting JSON values into text or binary formats, including syntax, parameters, and example implementations.

# Postgres json_serialize() Function

Convert JSON Values to Text or Binary Format

The `json_serialize()` function introduced in PostgreSQL 17 provides a flexible way to convert `JSON` values into text or binary format. Use it when you need to control the output format of `JSON` data or prepare it for transmission or storage in specific formats.

Use `json_serialize()` when you need to:

- Convert `JSON` values to specific text formats
- Transform `JSON` into binary representation
- Ensure consistent `JSON` string formatting
- Prepare `JSON` data for external systems or storage

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

## Function signature

The `json_serialize()` function uses the following syntax:

```sql
json_serialize(
    expression                              -- Input JSON expression
    [ FORMAT JSON [ ENCODING UTF8 ] ]       -- Optional input format specification
    [ RETURNING data_type                   -- Optional return type specification
      [ FORMAT JSON [ ENCODING UTF8 ] ] ]   -- Optional output format specification
) → text | bytea
```

Parameters:

- `expression`: Input `JSON` value or expression to serialize
- `FORMAT JSON`: Explicitly specifies `JSON` format for input (optional)
- `ENCODING UTF8`: Specifies `UTF8` encoding for input/output (optional)
- `RETURNING data_type`: Specifies the desired output type (optional, defaults to text)

## Example usage

Let's explore various ways to use the `json_serialize()` function with different inputs and output formats.

### Basic serialization

```sql
-- Serialize a simple JSON object to text
SELECT json_serialize('{"name": "Alice", "age": 30}');
```

```text
# |       json_serialize
--------------------------------
1 | {"name": "Alice", "age": 30}
```

```sql
-- Serialize a JSON array
SELECT json_serialize('[1, 2, 3, "four", true, null]');
```

```text
# |      json_serialize
----------------------------------
1 | [1, 2, 3, "four", true, null]
```

### Binary serialization

```sql
-- Convert JSON to binary format
SELECT json_serialize(
    '{"id": 1, "data": "test"}'
    RETURNING bytea
);
```

```text
# |                   json_serialize
--------------------------------------------------------
1 | \x7b226964223a20312c202264617461223a202274657374227d
```

### Working with complex structures

```sql
-- Serialize nested JSON structures
SELECT json_serialize('{
    "user": {
        "name": "Bob",
        "settings": {
            "theme": "dark",
            "notifications": true
        },
        "tags": ["admin", "active"]
    }
}');
```

```text
# |                                  json_serialize
---------------------------------------------------------------------------------------------------------------------
1 | { "user": { "name": "Bob", "settings": { "theme": "dark", "notifications": true }, "tags": ["admin", "active"] } }
```

## Comparison with `json()` function

While both `json_serialize()` and `json()` work with `JSON` data, they serve different purposes:

- `json()` converts text or binary data into `JSON` values
- `json_serialize()` converts `JSON` values into text or binary format
- `json()` focuses on input validation (for example, `WITH UNIQUE` keys)
- `json_serialize()` focuses on output format control

Think of them as complementary functions:

```sql
-- json() for input conversion
SELECT json('{"name": "Alice"}');  -- Text to JSON

-- json_serialize() for output conversion
SELECT json_serialize('{"name": "Alice"}'::json);  -- JSON to Text
```

## Common use cases

### Data export preparation

```sql
-- Create a table with JSON data
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    event_data json
);

-- Insert sample data
INSERT INTO events (event_data) VALUES
    ('{"type": "login", "user_id": 123}'),
    ('{"type": "purchase", "amount": 99.99}');

-- Export data in specific format
SELECT id, json_serialize(event_data RETURNING text)
FROM events;
```

## Error handling

The function handles various error conditions:

```sql
-- Invalid JSON input (raises error)
SELECT json_serialize('{"invalid": }');
```

```text
ERROR: invalid input syntax for type json (SQLSTATE 22P02)
```

## Learn more

- [json() function documentation](https://neon.com/docs/functions/json)
- [PostgreSQL JSON functions documentation](https://www.postgresql.org/docs/current/functions-json.html)
- [PostgreSQL data type formatting functions](https://www.postgresql.org/docs/current/functions-formatting.html)

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
