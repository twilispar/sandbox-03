> This page location: Tools & Workflows > API, CLI & SDKs > SDKs > Neon TypeScript SDK
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup and usage of the Neon TypeScript SDK for implementing authentication and database operations in applications, including various adapter options for different frameworks.

# Neon TypeScript SDK

Reference documentation for authentication and Data API database queries

This page documents `@neondatabase/neon-js`, which combines Neon Auth and the Data API in a single client. Neon also publishes standalone packages:

- [`@neondatabase/postgrest-js`](https://neon.com/docs/data-api/get-started#any-authentication-provider): Data API with any authentication provider
- [`@neondatabase/auth`](https://www.npmjs.com/package/@neondatabase/auth): Neon Auth without the Data API

Authentication is provided through an adapter-based architecture, letting you work more easily with your existing code or preferred framework. Available adapters:

- **BetterAuthVanillaAdapter** (default): Promise-based authentication methods like `client.auth.signIn.email()`. Used in all examples on this page.
- **BetterAuthReactAdapter**: Similar API but with React hooks like `useSession()`. See the [React quickstart](https://neon.com/docs/auth/quick-start/react).
- **SupabaseAuthAdapter**: Supabase-compatible API for easy migration. See the [migration guide](https://neon.com/docs/auth/migrate/from-supabase).

Database query methods (`client.from()`, `.select()`, etc.) work the same regardless of which adapter you use.

## Installation

Install the TypeScript SDK in your project using npm, yarn, pnpm, or bun.

```bash
npm install @neondatabase/neon-js
```

## Initialize the client

Method: `createClient(), createAuthClient()`

**Full client (`createClient`)**

Use this when you need both authentication and database queries. You get:

- Auth methods like `client.auth.signIn.email()` and `client.auth.signUp.email()`.
- Database queries like `client.from('todos').select()` and `client.from('users').insert()`.

**Auth-only client (`createAuthClient`)**

Use this when you only need authentication (no database queries). You get:

- Auth methods like `auth.signIn.email()` and `auth.signUp.email()`
- No database query methods

The auth methods are identical; only the access path differs. `client.auth.signIn.email()` and `auth.signIn.email()` do the same thing.

**Full client**

```typescript
import { createClient } from '@neondatabase/neon-js';

const client = createClient({
  auth: {
    url: import.meta.env.VITE_NEON_AUTH_URL,
  },
  dataApi: {
    url: import.meta.env.VITE_NEON_DATA_API_URL,
  },
});
```

**Auth-only**

```typescript
import { createAuthClient } from '@neondatabase/neon-js/auth';

const auth = createAuthClient(import.meta.env.VITE_NEON_AUTH_URL);
```

**With TypeScript types**

```typescript
import { createClient } from '@neondatabase/neon-js';
import type { Database } from './types/database.types';

const client = createClient<Database>({
  auth: {
    url: import.meta.env.VITE_NEON_AUTH_URL,
  },
  dataApi: {
    url: import.meta.env.VITE_NEON_DATA_API_URL,
  },
});
```

**With a different adapter**

```typescript
import { createClient } from '@neondatabase/neon-js';
import { BetterAuthReactAdapter } from '@neondatabase/neon-js/auth/react/adapters';

const client = createClient({
  auth: {
    adapter: BetterAuthReactAdapter(),
    url: import.meta.env.VITE_NEON_AUTH_URL,
  },
  dataApi: {
    url: import.meta.env.VITE_NEON_DATA_API_URL,
  },
});
```

## Create a new user account

Method: `auth.signUp.email()`

- Returns user and session data on success
- User data is stored in your database
- Sessions are managed automatically

### Parameters

<details>

<summary>View parameters</summary>

| Parameter   | Type                | Required |
| ----------- | ------------------- | -------- |
| email       | string              | ✓        |
| name        | string              | ✓        |
| password    | string              | ✓        |
| image       | string \| undefined |          |
| callbackURL | string \| undefined |          |

</details>

```typescript
const result = await client.auth.signUp.email({
  email: 'user@example.com',
  password: 'password123',
  name: 'John Doe'
})

if (result.error) {
console.error('Sign up error:', result.error.message)
} else {
console.log('User created:', result.data.user)
}

```

## Sign in with email and password

Method: `auth.signIn.email()`

- Returns user and session on success
- Session tokens are cached automatically
- Authentication state syncs across browser tabs

### Parameters

<details>

<summary>View parameters</summary>

| Parameter   | Type                 | Required |
| ----------- | -------------------- | -------- |
| email       | string               | ✓        |
| password    | string               | ✓        |
| rememberMe  | boolean \| undefined |          |
| callbackURL | string \| undefined  |          |

</details>

```typescript
const result = await client.auth.signIn.email({
  email: 'user@example.com',
  password: 'password123'
})

if (result.error) {
console.error('Sign in error:', result.error.message)
} else {
console.log('Signed in:', result.data.user.email)
}

```

## Sign in with OAuth provider

Method: `auth.signIn.social()`

Sign in with an OAuth provider like Google, GitHub, etc.

- Redirects user to provider's authorization page
- User is redirected back after authorization
- Session is created automatically

### Parameters

<details>

<summary>View parameters</summary>

| Parameter          | Type                   | Required |
| ------------------ | ---------------------- | -------- |
| provider           | string                 | ✓        |
| callbackURL        | string \| undefined    |          |
| newUserCallbackURL | string \| undefined    |          |
| errorCallbackURL   | string \| undefined    |          |
| disableRedirect    | boolean \| undefined   |          |
| idToken            | object                 |          |
| scopes             | string\[] \| undefined |          |
| requestSignUp      | boolean \| undefined   |          |
| loginHint          | string \| undefined    |          |
| additionalData     | object                 |          |

</details>

**Sign in with GitHub**

```typescript
await client.auth.signIn.social({
  provider: 'github',
  callbackURL: 'https://yourapp.com/auth/callback',
});
```

**Sign in with custom redirect**

```typescript
await client.auth.signIn.social({
  provider: 'google',
  callbackURL: 'https://yourapp.com/auth/callback',
});
```

## Sign out

Method: `auth.signOut()`

- Clears local session cache
- Notifies other browser tabs (cross-tab sync)
- Removes authentication tokens

```typescript
const { error } = await client.auth.signOut()

if (error) {
console.error('Sign out error:', error.message)
}

```

## Get current session

Method: `auth.getSession()`

- Returns cached session if available (fast)
- Automatically refreshes expired tokens
- Returns null if no active session

```typescript
const { data, error } = await client.auth.getSession()

if (data.session) {
console.log('User is logged in:', data.session.user.email)
} else {
console.log('No active session')
}

```

## Update user profile

Method: `auth.updateUser()`

Note: Password updates require password reset flow for security.

### Parameters

<details>

<summary>View parameters</summary>

| Parameter | Type                        | Required |
| --------- | --------------------------- | -------- |
| name      | string \| undefined         |          |
| image     | string \| null \| undefined |          |

</details>

```typescript
const { data, error } = await client.auth.updateUser({
  name: 'New Name'
})
```

## Send verification OTP code

Method: `auth.emailOtp.sendVerificationOtp()`

Sends an OTP (one-time password) code to the user's email for sign-in.
The user must then call `signIn.emailOtp()` with the received code.

### Parameters

<details>

<summary>View parameters</summary>

| Parameter | Type                                                   | Required |
| --------- | ------------------------------------------------------ | -------- |
| email     | string                                                 | ✓        |
| type      | "email-verification" \| "sign-in" \| "forget-password" | ✓        |

</details>

```typescript
const { error } = await client.auth.emailOtp.sendVerificationOtp({
  email: 'user@example.com',
  type: 'sign-in'
})

if (error) {
console.error('Failed to send OTP:', error.message)
}

```

## Sign in with OTP code

Method: `auth.signIn.emailOtp()`

Signs in a user using an OTP code received via email.
First call `emailOtp.sendVerificationOtp()` to send the code.

### Parameters

<details>

<summary>View parameters</summary>

| Parameter | Type   | Required |
| --------- | ------ | -------- |
| email     | string | ✓        |
| otp       | string | ✓        |

</details>

```typescript
const { data, error } = await client.auth.signIn.emailOtp({
  email: 'user@example.com',
  otp: '123456'
})

if (error) {
console.error('OTP verification failed:', error.message)
} else {
console.log('Signed in:', data.user.email)
}

```

## Verify email with OTP

Method: `auth.emailOtp.verifyEmail()`

Verifies a user's email address using an OTP code sent during signup.
This is typically used after `signUp.email()` when email verification is required.

### Parameters

<details>

<summary>View parameters</summary>

| Parameter | Type   | Required |
| --------- | ------ | -------- |
| email     | string | ✓        |
| otp       | string | ✓        |

</details>

```typescript
const { data, error } = await client.auth.emailOtp.verifyEmail({
  email: 'user@example.com',
  otp: '123456'
})

if (error) {
console.error('Email verification failed:', error.message)
} else {
console.log('Email verified successfully')
}

```

## Check verification OTP code

Method: `auth.emailOtp.checkVerificationOtp()`

Checks if an OTP code is valid without completing the verification flow.
Useful for password reset flows where you need to verify the code before allowing password change.

### Parameters

<details>

<summary>View parameters</summary>

| Parameter | Type                                                   | Required |
| --------- | ------------------------------------------------------ | -------- |
| email     | string                                                 | ✓        |
| type      | "email-verification" \| "sign-in" \| "forget-password" | ✓        |
| otp       | string                                                 | ✓        |

</details>

```typescript
const { data, error } = await client.auth.emailOtp.checkVerificationOtp({
  email: 'user@example.com',
  otp: '123456',
  type: 'forget-password'
})

if (error || !data.success) {
console.error('Invalid OTP code')
}

```

## Send verification email

Method: `auth.sendVerificationEmail()`

Sends a verification email to the user. Used for email verification after signup or email change.

### Parameters

<details>

<summary>View parameters</summary>

| Parameter   | Type                | Required |
| ----------- | ------------------- | -------- |
| email       | string              | ✓        |
| callbackURL | string \| undefined |          |

</details>

```typescript
const { error } = await client.auth.sendVerificationEmail({
  email: 'user@example.com',
  callbackURL: 'https://yourapp.com/verify-email'
})

if (error) {
console.error('Failed to send verification email:', error.message)
}

```

## Verify email address

Method: `auth.verifyEmail()`

Verifies an email address using a token from a verification email link.
Used for email change verification.

### Parameters

<details>

<summary>View parameters</summary>

| Parameter | Type   | Required |
| --------- | ------ | -------- |
| query     | object | ✓        |

</details>

```typescript
const { data, error } = await client.auth.verifyEmail({
  query: {
    token: 'verification-token-from-email',
    callbackURL: 'https://yourapp.com/email-verified'
  }
})

if (error) {
console.error('Email verification failed:', error.message)
}

```

## Request password reset

Method: `auth.requestPasswordReset()`

Sends a password reset email to the user. The email contains a link to reset the password.

### Parameters

<details>

<summary>View parameters</summary>

| Parameter  | Type                | Required |
| ---------- | ------------------- | -------- |
| email      | string              | ✓        |
| redirectTo | string \| undefined |          |

</details>

```typescript
const { error } = await client.auth.requestPasswordReset({
  email: 'user@example.com',
  redirectTo: 'https://yourapp.com/reset-password'
})

if (error) {
console.error('Failed to send password reset email:', error.message)
}

```

## Fetch data from a table

Method: `from().select()`

- Authentication token is included automatically if user is signed in
- Returns typed data based on your database schema
- Row-level security policies determine what data is returned

**Select all rows**

```typescript
const { data, error } = await client.from('todos').select('*');
```

**Select specific columns**

```typescript
const { data, error } = await client.from('todos').select('id, title, completed');
```

**Select with filter**

```typescript
const { data, error } = await client.from('todos').select('*').eq('completed', false);
```

**Select with related tables**

```typescript
const { data, error } = await client.from('todos').select('*, owner:users(*)');
```

## Insert data into a table

Method: `from().insert()`

- Authentication token is included automatically
- Can insert single or multiple rows
- Returns inserted data by default

**Insert a single row**

```typescript
const { data, error } = await client
  .from('todos')
  .insert({ title: 'Buy groceries', completed: false })
  .select();
```

**Insert multiple rows**

```typescript
const { data, error } = await client
  .from('todos')
  .insert([
    { title: 'Task 1', completed: false },
    { title: 'Task 2', completed: false },
  ])
  .select();
```

## Update existing rows

Method: `from().update()`

- Requires filter to specify which rows to update
- Authentication token is included automatically

```typescript
const { data, error } = await client
  .from('todos')
  .update({ completed: true })
  .eq('id', 1)
  .select()
```

## Delete rows from a table

Method: `from().delete()`

- Requires filter to specify which rows to delete
- Authentication token is included automatically

### Parameters

<details>

<summary>View parameters</summary>

| Parameter | Type                                             | Required |
| --------- | ------------------------------------------------ | -------- |
| count     | "exact" \| "planned" \| "estimated" \| undefined |          |

</details>

```typescript
const { error } = await client
  .from('todos')
  .delete()
  .eq('id', 1)
```

## Call a stored procedure

Method: `.rpc()`

- Authentication token is included automatically
- Pass parameters as object
- Returns function result

```typescript
const { data, error } = await client.rpc('get_user_stats', {
  user_id: 123,
  start_date: '2024-01-01'
})

if (error) {
console.error('RPC error:', error.message)
} else {
console.log('Stats:', data)
}

```

## Column is equal to a value

Method: `.eq(column, value)`

Filters rows where the specified column equals the given value.
Can be chained with other filters to create complex queries.

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .eq('completed', true)
```

## Column is not equal to a value

Method: `.neq(column, value)`

Filters rows where the specified column does not equal the given value.
Useful for excluding specific values from results.

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .neq('status', 'archived')
```

## Column is greater than a value

Method: `.gt(column, value)`

Filters rows where the specified column is greater than the given value.
Works with numeric values, dates, and other comparable types.

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .gt('priority', 5)
```

## Column is less than a value

Method: `.lt(column, value)`

Filters rows where the specified column is less than the given value.
Works with numeric values, dates, and other comparable types.

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .lt('priority', 10)
```

## Order results by column

Method: `.order(column, options)`

Sorts query results by the specified column.
Use `{ ascending: true }` for ascending order or `{ ascending: false }` for descending order.

**Order ascending**

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .order('created_at', { ascending: true });
```

**Order descending**

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .order('created_at', { ascending: false });
```

## Limit number of results

Method: `.limit(count)`

Limits the number of rows returned by the query.
Useful for pagination and preventing large result sets.

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .limit(10)
```

## Column is greater than or equal to a value

Method: `.gte(column, value)`

Filters rows where the specified column is greater than or equal to the given value.
The comparison is inclusive (includes rows where column equals the value).

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .gte('priority', 5)
```

## Column is less than or equal to a value

Method: `.lte(column, value)`

Filters rows where the specified column is less than or equal to the given value.
The comparison is inclusive (includes rows where column equals the value).

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .lte('priority', 10)
```

## Column matches a pattern

Method: `.like(column, pattern)`

Filter rows where column matches pattern (case-sensitive).

Use % as wildcard: '%pattern%' matches any string containing 'pattern'

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .like('title', '%groceries%')
```

## Column matches a pattern (case-insensitive)

Method: `.ilike(column, pattern)`

Use % as wildcard: '%pattern%' matches any string containing 'pattern'

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .ilike('title', '%groceries%')
```

## Column is null or not null

Method: `.is(column, value)`

Filters rows based on whether a column is null or not null.
Use `null` to find rows where the column is null, or `'not.null'` to find rows where it's not null.

**Is null**

```typescript
const { data, error } = await client.from('todos').select('*').is('deleted_at', null);
```

**Is not null**

```typescript
const { data, error } = await client.from('todos').select('*').is('completed_at', 'not.null');
```

## Column value is in an array

Method: `.in(column, array)`

Filters rows where the column value matches any value in the provided array.
Useful for filtering by multiple possible values (for example, status in ['pending', 'active']).

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .in('status', ['pending', 'in-progress'])
```

## Array or JSONB column contains value

Method: `.contains(column, value)`

Filters rows where an array or JSONB column contains the specified value.
For arrays, checks if the value exists in the array. For JSONB, checks if the value is contained in the JSON object.

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .contains('tags', ['urgent'])
```

## Column value is between two values

Method: `.range(column, start, end)`

Range is inclusive (includes both start and end values).

```typescript
const { data, error } = await client
  .from('todos')
  .select('*')
  .range('priority', 5, 10)
```

## Auth method differences by adapter

Each adapter exposes a different API surface for authentication. Using the wrong method for your adapter is a common source of errors.

| Adapter                  | Sign in                                        | Sign up                                  |
| ------------------------ | ---------------------------------------------- | ---------------------------------------- |
| BetterAuthVanillaAdapter | `auth.signIn.email({ email, password })`       | `auth.signUp.email({ email, password })` |
| BetterAuthReactAdapter   | `auth.signIn.email({ email, password })`       | `auth.signUp.email({ email, password })` |
| SupabaseAuthAdapter      | `auth.signInWithPassword({ email, password })` | `auth.signUp({ email, password })`       |

For common authentication troubleshooting (missing environment variables, CSS conflicts, wrong imports), see [Auth troubleshooting](https://neon.com/docs/auth/troubleshooting).

---

## Related docs (SDKs)

- [Overview](https://neon.com/docs/reference/sdk)
- [Neon API TypeScript SDK](https://neon.com/docs/reference/typescript-sdk)
- [Python SDK (Neon API)](https://neon.com/docs/reference/python-sdk)
- [The @neondatabase/toolkit](https://neon.com/docs/reference/neondatabase-toolkit)
- [Go SDK (Neon API)](https://neon.com/docs/https://github.com/kislerdm/neon-sdk-go)
- [Node.js / Deno SDK (Neon API)](https://neon.com/docs/https://github.com/paambaati/neon-js-sdk)
