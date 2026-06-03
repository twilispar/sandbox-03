> This page location: AI > AI for Agents > MCP integration > Overview
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup and management of Neon Postgres databases using the Neon MCP Server, enabling users to execute commands and make schema changes through natural language without coding.

# Neon MCP Server overview

Learn about managing your Neon projects using natural language with Neon MCP Server

The **Neon MCP Server** is an open-source tool that lets you interact with your Neon Postgres databases in **natural language**:

- Manage projects, branches, and databases with conversational commands
- Run SQL queries and make schema changes without writing code
- Use branch-based migrations for safer schema modifications

## Quick setup

The fastest way to set up Neon's MCP Server is with one command:

```bash
npx neonctl@latest init
```

This configures the Neon MCP Server for compatible MCP clients in your workspace using API key authentication, including Cursor, VS Code, Claude Code, and other assistants [add-mcp can target](https://neon.com/docs/ai/connect-mcp-clients-to-neon#supported-agents-add-mcp). See the [neonctl init documentation](https://neon.com/docs/reference/cli-init).

**If you only want the MCP server and nothing else**, use:

```bash
npx add-mcp https://mcp.neon.tech/mcp
```

This command adds the required configuration to your editor's MCP config files; it does not open a browser by itself. Add `-g` for global (user-level) setup instead of project-level. Restart your editor (or enable the MCP server in your editor's settings). When you use the MCP connection, an OAuth window will open in your browser to authorize access to your Neon account. For more options (for example, global vs project-level), see the [add-mcp repository](https://github.com/neondatabase/add-mcp).

**Other setup options:**

- **API key authentication (remote agents):** For remote agents or when OAuth isn't available:

  ```bash
  npx add-mcp https://mcp.neon.tech/mcp --header 'Authorization: Bearer ${NEON_API_KEY}'
  ```

- **Manual configuration:** See [Connect MCP clients](https://neon.com/docs/ai/connect-mcp-clients-to-neon) for step-by-step instructions for any editor, including Windsurf, ChatGPT, Zed, and others.

After setup, restart your editor and ask your AI assistant to **"Get started with Neon"** to launch the interactive onboarding guide.

---

Imagine you want to create a new database. Instead of using the Neon Console or API, you could just type a request like, "Create a database named 'my-new-database'". Or, to see your projects, you might ask, "List all my Neon projects". The Neon MCP Server makes this possible.

It works by acting as a bridge between natural language requests and the [Neon API](https://api-docs.neon.tech/reference/getting-started-with-neon-api). Built upon the [Model Context Protocol (MCP)](https://modelcontextprotocol.org), it translates your requests into the necessary Neon API calls, allowing you to manage everything from creating projects and branches to running queries and performing database migrations.

**Important: Neon MCP Server Security Considerations**

The Neon MCP Server grants powerful database management capabilities through natural language requests. **Always review and authorize actions requested by the LLM before execution.** Ensure that only authorized users and applications have access to the Neon MCP Server.

## Other setup options

### MCP Server Config Generator

Use this generator to build valid Neon hosted MCP config snippets with supported auth modes, transport, and headers:

## Supported actions (tools)

The Neon MCP Server provides the following actions, which are exposed as "tools" to MCP clients. You can use these tools to interact with your Neon projects and databases using natural language commands.

**Project management:**

- `list_projects`: Lists the first 10 Neon projects in your account, providing a summary of each project. If you can't find a specific project, increase the limit by passing a higher value to the limit parameter.
- `list_shared_projects`: Lists Neon projects shared with the current user. Supports a search parameter and limiting the number of projects returned (default: 10).
- `describe_project`: Fetches detailed information about a specific Neon project, including its ID, name, and associated branches and databases.
- `create_project`: Creates a new Neon project in your Neon account. A project acts as a container for branches, databases, roles, and computes.
- `delete_project`: Deletes an existing Neon project and all its associated resources.
- `list_organizations`: Lists all organizations that the current user has access to. Optionally filter by organization name or ID using the search parameter.

**Branch management:**

- `create_branch`: Creates a new branch within a specified Neon project. By default the branch is created from the project's default branch; pass optional `parentId` (a branch ID such as `br-...`) to fork from an existing non-default branch instead. Leverages [Neon's branching](https://neon.com/docs/introduction/branching) feature for development, testing, or migrations.
- `delete_branch`: Deletes an existing branch from a Neon project.
- `describe_branch`: Retrieves details about a specific branch, such as its name, ID, and parent branch.
- `list_branch_computes`: Lists compute endpoints for a project or specific branch, including compute ID, type, size, last active time, and autoscaling information.
- `compare_database_schema`: Shows the schema diff between the child branch and its parent.
- `reset_from_parent`: Resets the current branch to its parent's state, discarding local changes. Automatically preserves to backup if branch has children, or optionally preserve on request with a custom name.

**SQL query execution:**

- `get_connection_string`: Returns your database connection string.
- `run_sql`: Executes a single SQL query against a specified Neon database. Supports both read and write operations.
- `run_sql_transaction`: Executes a series of SQL queries within a single transaction against a Neon database.
- `get_database_tables`: Lists all tables within a specified Neon database.
- `describe_table_schema`: Retrieves the schema definition of a specific table, detailing columns, data types, and constraints.

**Database migrations (schema changes):**

- `prepare_database_migration`: Initiates a database migration process. Critically, it creates a temporary branch to apply and test the migration safely before affecting the main branch.
- `complete_database_migration`: Finalizes and applies a prepared database migration to the main branch. This action merges changes from the temporary migration branch and cleans up temporary resources.

**SQL querying and optimization:**

- `list_slow_queries`: Identifies performance bottlenecks by finding the slowest queries in a database. Requires the pg_stat_statements extension.
- `explain_sql_statement`: Provides detailed execution plans for SQL queries to help identify performance bottlenecks.
- `prepare_query_tuning`: Analyzes query performance and suggests optimizations, like index creation. Creates a temporary branch for safely testing these optimizations.
- `complete_query_tuning`: Finalizes query tuning by either applying optimizations to the main branch or discarding them. Cleans up the temporary tuning branch.

**Neon Auth:**

To set up Neon Auth in your application code, use [Agent Skills](https://neon.com/docs/ai/agent-skills) after running `npx neonctl@latest init`. See [Set up Neon Auth with your AI editor](https://neon.com/docs/auth/overview#set-up-with-your-ai-editor).

- `provision_neon_auth`: Provisions [Neon Auth](https://neon.com/docs/auth/overview) for a project's branch so you can add authentication to your app without standing up a separate auth service. If Neon Auth is already provisioned, this returns the existing configuration instead of erroring.
- `get_neon_auth_config`: Returns the Neon Auth configuration for a branch, including the auth `base_url`, `jwks_url`, trusted origins, allowed auth methods (for example email and password), configured OAuth providers, and email provider settings. Secrets such as OAuth client secrets and SMTP passwords are redacted.
- `configure_neon_auth`: Updates the Neon Auth configuration for a branch. Use it to manage trusted origins, toggle localhost, enable or tune email and password sign-in, add or update OAuth providers, configure the email provider, and send a test email.

**Neon Data API:**

- `provision_neon_data_api`: Provisions the Neon Data API for a branch, enabling HTTP-based Data API access with optional JWT authentication.

**Search and discovery:**

- `search`: Searches across organizations, projects, and branches matching a query. Returns IDs, titles, and direct links to the Neon Console.
- `fetch`: Fetches detailed information about a specific organization, project, or branch using an ID (typically from the search tool).

In project-scoped mode, `search` and `fetch` are not available.

**Documentation and resources:**

- `list_docs_resources`: Lists all available Neon documentation pages by fetching the docs index. Returns page URLs and titles that can be fetched individually using the `get_doc_resource` tool.
- `get_doc_resource`: Fetches a specific Neon documentation page as markdown content. Use the `list_docs_resources` tool first to discover available page slugs, then pass the slug to this tool.

### Troubleshooting

If your client does not use JSON for configuration of MCP servers (such as older versions of Cursor), use this command when prompted:

```bash
npx -y @neondatabase/mcp-server-neon start <YOUR_NEON_API_KEY>
```

**Note:** For clients that don't support Streamable HTTP, you can use the deprecated SSE endpoint: `https://mcp.neon.tech/sse`. SSE is not supported with API key authentication.

## Usage examples

After setup, interact with your Neon databases using natural language:

- `"Get started with Neon"`: Launch the interactive onboarding guide
- `"List my Neon projects"`
- `"Create a project named 'my-app'"`
- `"Show tables in database 'main'"`
- `"Search for 'production' across my Neon resources"`
- `"SELECT * FROM users LIMIT 10"`

## MCP security guidance

The Neon MCP server provides powerful database tools. We recommend MCP for **development and testing only**, not production environments.

- Use MCP only for local development or IDE-based workflows
- Never connect MCP agents to production databases
- Avoid exposing production or PII data; use anonymized data only
- Always review and authorize LLM-requested actions before execution
- Restrict MCP access to trusted users and regularly audit access

## Resources

- [MCP Protocol](https://modelcontextprotocol.org)
- [Neon API Reference](https://api-docs.neon.tech/reference/getting-started-with-neon-api)
- [Neon API Keys](https://neon.com/docs/manage/api-keys#creating-api-keys)
- [Neon MCP server GitHub](https://github.com/neondatabase/mcp-server-neon)

---

## Related docs (MCP integration)

- [Connect MCP clients](https://neon.com/docs/ai/connect-mcp-clients-to-neon)
