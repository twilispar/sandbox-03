> This page location: Backend > Neon Auth > Guides > Manage Auth via the API
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: How to enable, retrieve, and disable Neon Auth on a branch using the Neon REST API, including curl examples and response reference.

# Manage Neon Auth via the API

Enable, configure, and disable Neon Auth using the Neon API

**Note: Beta**

The **Neon Auth with Better Auth** is in Beta. Share your feedback on [Discord](https://discord.gg/92vNTzKDGp) or via the [Neon Console](https://console.neon.tech/app/projects?modal=feedback).

You can manage Neon Auth programmatically using the [Neon API](https://api-docs.neon.tech/reference/getting-started). You can also enable and configure Neon Auth from an AI editor using the [Neon MCP server](https://neon.com/docs/ai/neon-mcp-server#supported-actions-tools) (`provision_neon_auth`, `configure_neon_auth`, `get_neon_auth_config`). See [Set up with your AI editor](https://neon.com/docs/auth/overview#set-up-with-your-ai-editor).

**Note:** Neon Auth operates at the **branch level**. Each branch can have its own independent auth configuration, which means preview and development branches can have separate auth state from your production branch.

## Prerequisites

- A [Neon API key](https://neon.com/docs/manage/api-keys)
- A Neon project with at least one branch

All requests use the base URL `https://console.neon.tech/api/v2` and require the `Authorization: Bearer $NEON_API_KEY` header. The `project_id` and `branch_id` values are returned when you [create a project](https://neon.com/docs/manage/projects#create-a-project-with-the-api) or [list branches](https://neon.com/docs/manage/branches#list-branches-with-the-api) via the API.

## Enable Neon Auth

Send a `POST` request to enable Neon Auth on a branch:

```bash
curl -X POST 'https://console.neon.tech/api/v2/projects/{project_id}/branches/{branch_id}/auth' \
  -H 'Authorization: Bearer $NEON_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{"auth_provider": "better_auth"}'
```

Response (201 Created):

```json
{
  "auth_provider": "better_auth",
  "auth_provider_project_id": "cab6949a-10e3-4d25-a879-512beed281e3",
  "pub_client_key": "",
  "secret_server_key": "",
  "jwks_url": "https://ep-example.neonauth.us-east-1.aws.neon.tech/neondb/auth/.well-known/jwks.json",
  "schema_name": "neon_auth",
  "table_name": "users_sync",
  "base_url": "https://ep-example.neonauth.us-east-1.aws.neon.tech/neondb/auth"
}
```

The response includes:

| Field                      | Description                                                                                               |
| -------------------------- | --------------------------------------------------------------------------------------------------------- |
| `auth_provider`            | The configured provider (`better_auth`)                                                                   |
| `auth_provider_project_id` | Unique ID for the auth provider instance                                                                  |
| `pub_client_key`           | Public client key (shown once at creation, may be empty for `better_auth`)                                |
| `secret_server_key`        | Secret server key (shown once at creation, may be empty for `better_auth`)                                |
| `jwks_url`                 | JWKS endpoint for JWT verification                                                                        |
| `schema_name`              | Database schema created for auth tables (`neon_auth`)                                                     |
| `table_name`               | Table name for synced user data (`users_sync`)                                                            |
| `base_url`                 | Base URL of the auth service, used for SDK configuration and the interactive API reference (`/reference`) |

**Important:** The enable response is the only time the API returns `pub_client_key` and `secret_server_key`. Store them securely. Subsequent `GET` requests do not include these fields.

If Neon Auth is already enabled on the branch, this call returns an error.

**Tip: Using a non-default database**

By default, Neon Auth uses the branch's default database. To target a different database, add `database_name` to the request body: `{"auth_provider": "better_auth", "database_name": "my_other_db"}`

## Get Auth configuration

Retrieve the current Neon Auth configuration for a branch:

```bash
curl -X GET 'https://console.neon.tech/api/v2/projects/{project_id}/branches/{branch_id}/auth' \
  -H 'Authorization: Bearer $NEON_API_KEY'
```

Response (200 OK):

```json
{
  "auth_provider": "better_auth",
  "auth_provider_project_id": "cab6949a-10e3-4d25-a879-512beed281e3",
  "branch_id": "br-example-abc123",
  "db_name": "neondb",
  "created_at": "2026-02-26T04:29:05Z",
  "owned_by": "neon",
  "jwks_url": "https://ep-example.neonauth.us-east-1.aws.neon.tech/neondb/auth/.well-known/jwks.json",
  "base_url": "https://ep-example.neonauth.us-east-1.aws.neon.tech/neondb/auth",
  "name": "My App"
}
```

## Update auth configuration

Update auth settings for a branch. Currently supports changing the application name shown in user-facing auth messages. Applies to Neon Auth (Better Auth) integrations only. Defaults to the Neon project name.

```bash
curl -X PATCH 'https://console.neon.tech/api/v2/projects/{project_id}/branches/{branch_id}/auth/config' \
  -H 'Authorization: Bearer $NEON_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{"name": "My App"}'
```

Response (200 OK):

```json
{
  "name": "My App"
}
```

| Field  | Description                                                    |
| ------ | -------------------------------------------------------------- |
| `name` | The name shown in user-facing auth messages (1-256 characters) |

Each branch manages its own application name independently. You can also update this from the **Auth** page > **Configuration** tab > **Project Info** panel in the Neon Console.

## Disable Neon Auth

Send a `DELETE` request to disable Neon Auth on a branch:

```bash
curl -X DELETE 'https://console.neon.tech/api/v2/projects/{project_id}/branches/{branch_id}/auth' \
  -H 'Authorization: Bearer $NEON_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{"delete_data": true}'
```

Response (200 OK): Empty body.

The `delete_data` field controls whether the system removes the `neon_auth` schema from your database:

- **`true`**: Deletes the `neon_auth` schema and all auth tables (users, sessions, accounts).
- **`false`** (default): Disables the auth service but leaves the schema and data intact. You can re-enable later without losing user data.

**Warning:** Setting `delete_data` to `true` permanently removes all auth data from the database. You cannot undo this.

## Related auth endpoints

The Neon API also provides endpoints for managing auth configuration at the branch level. These are available at `https://console.neon.tech/api/v2/projects/{project_id}/branches/{branch_id}/auth/...`:

| Endpoint                | Methods                  | Description                                                                                 |
| ----------------------- | ------------------------ | ------------------------------------------------------------------------------------------- |
| `/domains`              | GET, POST, DELETE        | Manage trusted redirect domains                                                             |
| `/oauth_providers`      | GET, POST, PATCH, DELETE | Configure OAuth providers (Google, GitHub, etc.)                                            |
| `/email_provider`       | GET, PATCH               | Configure the email provider                                                                |
| `/email_and_password`   | GET, PATCH               | Configure email/password authentication                                                     |
| `/users`                | POST, DELETE, PUT        | Create, delete, and manage user roles                                                       |
| `/plugins`              | GET, PATCH               | View and configure [auth plugins](https://neon.com/docs/auth/guides/plugins)                |
| `/plugins/magic-link`   | PATCH                    | Configure the [Magic Link plugin](https://neon.com/docs/auth/guides/plugins/magic-link)     |
| `/plugins/phone-number` | GET, PATCH               | Configure the [Phone Number plugin](https://neon.com/docs/auth/guides/plugins/phone-number) |
| `/webhooks`             | GET, PUT                 | Configure webhook notifications                                                             |
| `/allow_localhost`      | GET, PATCH               | Toggle localhost access for development                                                     |
| `/config`               | PATCH                    | Update auth configuration (application name)                                                |
| `/send_test_email`      | POST                     | Send a test email to verify email configuration                                             |

For full request/response details on these endpoints, see the [interactive API reference](https://api-docs.neon.tech/reference/getting-started).

**Tip: TypeScript SDK**

You can also manage Neon Auth using the [Neon TypeScript SDK](https://neon.com/docs/reference/typescript-sdk).

---

## Related docs (Guides)

- [Email verification](https://neon.com/docs/auth/guides/email-verification)
- [Set up OAuth](https://neon.com/docs/auth/guides/setup-oauth)
- [Password reset](https://neon.com/docs/auth/guides/password-reset)
- [User management](https://neon.com/docs/auth/guides/user-management)
- [Configure domains](https://neon.com/docs/auth/guides/configure-domains)
- [Webhooks](https://neon.com/docs/auth/guides/webhooks)
- [Production checklist](https://neon.com/docs/auth/production-checklist)
- [Troubleshooting](https://neon.com/docs/auth/troubleshooting)
