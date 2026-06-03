> This page location: Postgres guides > Functions > JSON functions > json_table
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the usage of the `JSON_TABLE` function in PostgreSQL 17 to transform JSON data into relational views, enabling SQL queries on complex JSON structures through a virtual table format.

# Postgres JSON_TABLE() function

Transform JSON data into relational views

The `JSON_TABLE` function transforms JSON data into relational views, allowing you to query JSON data using standard SQL operations. Added in PostgreSQL 17, this feature helps you work with complex JSON data by presenting it as a virtual table which you can access with regular SQL queries.

Use `JSON_TABLE` when you need to:

- Extract specific fields from complex JSON structures
- Convert JSON arrays into rows
- Join JSON data with regular tables
- Apply SQL operations like filtering and aggregation to JSON data

> **Try it on Neon!**
>
> Neon is Serverless Postgres built for the cloud. Explore Postgres features and functions in our user-friendly SQL editor. Sign up for a free account to get started.
>
> [Sign Up](https://console.neon.tech/signup)

## Function signature

`JSON_TABLE` uses the following syntax:

```sql
JSON_TABLE(
    json_doc,           -- JSON/JSONB input
    path_expression     -- SQL/JSON path expression
    COLUMNS (
        column_definition [, ...]
    )
) AS alias
```

Parameters:

- `json_doc`: JSON or JSONB data to process
- `path_expression`: SQL/JSON path expression that identifies rows to generate
- `COLUMNS`: Defines the schema of the virtual table
- `column_definition`: Specifies how to extract values for each column
- `alias`: Name for the resulting virtual table

## Example usage

Let's explore `JSON_TABLE` using a library management system example. We'll store book information including reviews, borrowing history, and metadata in JSON format.

### Create a test database

```sql
-- Test database table for a library management system
CREATE TABLE library_books (
    book_id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    data JSONB NOT NULL
);

-- Insert sample data
INSERT INTO library_books (title, data) VALUES
(
    'The Art of Programming',
    '{
        "isbn": "978-0123456789",
        "author": {
            "name": "Jane Smith",
            "email": "jane.smith@example.com"
        },
        "publication": {
            "year": 2023,
            "publisher": "Tech Books Inc"
        },
        "metadata": {
            "genres": ["Programming", "Computer Science"],
            "tags": ["algorithms", "python", "best practices"],
            "edition": "2nd"
        },
        "reviews": [
            {
                "user": "john_doe",
                "rating": 5,
                "comment": "Excellent book for beginners!",
                "date": "2024-01-15"
            },
            {
                "user": "mary_jane",
                "rating": 4,
                "comment": "Good examples, could use more exercises",
                "date": "2024-02-20"
            }
        ],
        "borrowing_history": [
            {
                "user_id": "U123",
                "checkout_date": "2024-01-01",
                "return_date": "2024-01-15",
                "condition": "good"
            },
            {
                "user_id": "U456",
                "checkout_date": "2024-02-01",
                "return_date": "2024-02-15",
                "condition": "fair"
            }
        ]
    }'::jsonb
),
(
    'Database Design Fundamentals',
    '{
        "isbn": "978-0987654321",
        "author": {
            "name": "Robert Johnson",
            "email": "robert.j@example.com"
        },
        "publication": {
            "year": 2024,
            "publisher": "Database Press"
        },
        "metadata": {
            "genres": ["Database", "Computer Science"],
            "tags": ["SQL", "design patterns", "normalization"],
            "edition": "1st"
        },
        "reviews": [
            {
                "user": "alice_wonder",
                "rating": 5,
                "comment": "Comprehensive coverage of database concepts",
                "date": "2024-03-01"
            }
        ],
        "borrowing_history": [
            {
                "user_id": "U789",
                "checkout_date": "2024-03-01",
                "return_date": null,
                "condition": "excellent"
            }
        ]
    }'::jsonb
);
```

### Query examples

#### Extract basic book information

This query extracts core book details from the JSON structure into a relational format.

```sql
SELECT b.book_id, b.title, jt.*
FROM library_books b,
JSON_TABLE(
    data,
    '$'
    COLUMNS (
        isbn text PATH '$.isbn',
        author_name text PATH '$.author.name',
        publisher text PATH '$.publication.publisher',
        pub_year int PATH '$.publication.year'
    )
) AS jt;
```

Result:

| book\_id | title                        | isbn           | author\_name   | publisher      | pub\_year |
| -------- | ---------------------------- | -------------- | -------------- | -------------- | --------- |
| 1        | The Art of Programming       | 978-0123456789 | Jane Smith     | Tech Books Inc | 2023      |
| 2        | Database Design Fundamentals | 978-0987654321 | Robert Johnson | Database Press | 2024      |

#### Analyze book reviews

This query flattens the reviews array into rows, making it easy to analyze reader feedback.

```sql
SELECT
    b.title,
    jt.*
FROM library_books b,
JSON_TABLE(
    data,
    '$.reviews[*]'
    COLUMNS (
        reviewer text PATH '$.user',
        rating int PATH '$.rating',
        review_date date PATH '$.date',
        comment text PATH '$.comment'
    )
) AS jt
ORDER BY review_date DESC;
```

Result:

| title                        | reviewer      | rating | review\_date | comment                                     |
| ---------------------------- | ------------- | ------ | ------------ | ------------------------------------------- |
| Database Design Fundamentals | alice\_wonder | 5      | 2024-03-01   | Comprehensive coverage of database concepts |
| The Art of Programming       | mary\_jane    | 4      | 2024-02-20   | Good examples, could use more exercises     |
| The Art of Programming       | john\_doe     | 5      | 2024-01-15   | Excellent book for beginners!               |

#### Track borrowing history

This query helps track book loans and current borrowing status.

```sql
WITH book_loans AS (
    SELECT
        b.title,
        jt.*
    FROM library_books b,
    JSON_TABLE(
        data,
        '$.borrowing_history[*]'
        COLUMNS (
            user_id text PATH '$.user_id',
            checkout_date date PATH '$.checkout_date',
            return_date date PATH '$.return_date',
            condition text PATH '$.condition'
        )
    ) AS jt
)
SELECT
    title,
    user_id,
    checkout_date,
    COALESCE(return_date::text, 'Still borrowed') as return_status,
    condition
FROM book_loans
ORDER BY checkout_date DESC;
```

Result:

| title                        | user\_id | checkout\_date | return\_status | condition |
| ---------------------------- | -------- | -------------- | -------------- | --------- |
| Database Design Fundamentals | U789     | 2024-03-01     | Still borrowed | excellent |
| The Art of Programming       | U456     | 2024-02-01     | 2024-02-15     | fair      |
| The Art of Programming       | U123     | 2024-01-01     | 2024-01-15     | good      |

### Advanced usage

#### Aggregate review data

Use this query to calculate review statistics for each book.

```sql
WITH book_ratings AS (
    SELECT
        b.title,
        jt.rating
    FROM library_books b,
    JSON_TABLE(
        data,
        '$.reviews[*]'
        COLUMNS (
            rating int PATH '$.rating'
        )
    ) AS jt
)
SELECT
    title,
    COUNT(*) as num_reviews,
    ROUND(AVG(rating), 2) as avg_rating,
    MIN(rating) as min_rating,
    MAX(rating) as max_rating
FROM book_ratings
GROUP BY title;
```

Result

| title                        | num\_reviews | avg\_rating | min\_rating | max\_rating |
| ---------------------------- | ------------ | ----------- | ----------- | ----------- |
| Database Design Fundamentals | 1            | 5.00        | 5           | 5           |
| The Art of Programming       | 2            | 4.50        | 4           | 5           |

#### Process arrays and metadata

This query extracts array fields and metadata into queryable columns.

```sql
SELECT
    b.title,
    jt.*
FROM library_books b,
JSON_TABLE(
    data,
    '$'
    COLUMNS (
        genres json FORMAT JSON PATH '$.metadata.genres',
        tags json FORMAT JSON PATH '$.metadata.tags',
        edition text PATH '$.metadata.edition'
    )
) AS jt;
```

Result:

| title                        | genres                               | tags                                         | edition |
| ---------------------------- | ------------------------------------ | -------------------------------------------- | ------- |
| The Art of Programming       | \["Programming", "Computer Science"] | \["algorithms", "python", "best practices"]  | 2nd     |
| Database Design Fundamentals | \["Database", "Computer Science"]    | \["SQL", "design patterns", "normalization"] | 1st     |

## Error handling

`JSON_TABLE` returns NULL for missing values by default. You can modify this behavior with error handling clauses:

```sql
SELECT title, jt.*
FROM library_books,
JSON_TABLE(
    data,
    '$'
    COLUMNS (
        author_name text PATH '$.author.name',
        metadata TEXT PATH '$.metadata' DEFAULT '{}' ON ERROR,
        edition text PATH '$.metadata.edition' DEFAULT 'Unknown' ON EMPTY DEFAULT 'Unknown' ON ERROR
    )
) AS jt;
```

This example shows how to handle errors when extracting JSON data. There is an error here because the `metadata` field is not of type `TEXT`.

| title                        | author\_name   | metadata | edition |
| ---------------------------- | -------------- | -------- | ------- |
| The Art of Programming       | Jane Smith     | \{}      | 2nd     |
| Database Design Fundamentals | Robert Johnson | \{}      | 1st     |

## Performance tips

1. Create GIN indexes on JSONB columns:

   ```sql
   CREATE INDEX idx_library_books_data ON library_books USING GIN (data);
   ```

2. Consider these optimizations:
   - Place filters on regular columns before JSON operations
   - Use JSON operators (`->`, `->>`, `@>`) when possible
   - Materialize frequently accessed JSON paths into regular columns
   - Break large JSON documents into smaller pieces to manage memory usage

## Learn more

- [PostgreSQL JSON_TABLE documentation](https://www.postgresql.org/docs/current/functions-json.html#FUNCTIONS-SQLJSON-TABLE)
- [PostgreSQL JSON functions](https://www.postgresql.org/docs/current/functions-json.html)

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
- [json_to_record](https://neon.com/docs/functions/json_to_record)
- [json_value](https://neon.com/docs/functions/json_value)
- [jsonb_array_elements](https://neon.com/docs/functions/jsonb_array_elements)
- [jsonb_each](https://neon.com/docs/functions/jsonb_each)
- [jsonb_extract_path](https://neon.com/docs/functions/jsonb_extract_path)
- [jsonb_extract_path_text](https://neon.com/docs/functions/jsonb_extract_path_text)
- [jsonb_object](https://neon.com/docs/functions/jsonb_object)
- [jsonb_populate_record](https://neon.com/docs/functions/jsonb_populate_record)
- [jsonb_to_record](https://neon.com/docs/functions/jsonb_to_record)
