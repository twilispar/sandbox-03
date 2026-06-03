> This page location: Community > Component specialized guide
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the use of specialized MDX components in Neon for specific scenarios, detailing when to select them, their technical requirements, and best practices for implementation.

# Component specialized guide

Specialized and less common components for specific use cases

A comprehensive reference for specialized and less commonly used MDX components in Neon documentation. This guide covers components used in specific scenarios, specialized workflows, and edge cases.

**Note: Component status**

Some components in this guide are currently unused or may be deprecated in future updates. These are included for reference but may be removed from the codebase.

**What you will learn:**

- How to use specialized MDX components for specific use cases
- When to choose specialized components over common ones
- Technical requirements and dependencies for specialized components
- Best practices for complex component implementations

**Related topics**

- [Component Guide](https://neon.com/docs/community/component-guide)
- [Component Icon Guide](https://neon.com/docs/community/component-icon-guide)
- [Component Architecture](https://neon.com/docs/community/component-architecture)
- [Documentation Contribution Guide](https://neon.com/docs/community/contribution-guide)

## Quick navigation

- [Code display](https://neon.com/docs/community/component-specialized#code-display) - Enhanced code blocks and external code
- [Media components](https://neon.com/docs/community/component-specialized#media-components) - Video and multimedia content
- [Specialized shared components](https://neon.com/docs/community/component-specialized#specialized-shared-components) - Feature status indicators
- [SDK components](https://neon.com/docs/community/component-specialized#sdk-components) - Auto-generated SDK documentation
- [Utility components](https://neon.com/docs/community/component-specialized#utility-components) - Forms and specialized UI elements
- [Specialized components](https://neon.com/docs/community/component-specialized#specialized-components) - Complex and specialized functionality
- [Community components](https://neon.com/docs/community/component-specialized#community-components) - Community engagement features

---

## Code display

Enhanced code blocks and external code embedding.

### ExternalCode

Embed code from external sources or files.

```mdx
<ExternalCode url="https://raw.githubusercontent.com/neondatabase/neon/master/README.md" />
```

**Live preview:**

_[External code loaded from GitHub README.md with syntax highlighting]_

Example of external code loading (mocked for showcase):

```markdown
# Neon Database

Serverless Postgres built for the cloud.

## Key Features

- **Instant provisioning**: Create databases in seconds
- **Autoscaling**: Scale compute up and down automatically
- **Branching**: Create database branches like Git
- **Scale to zero**: Save costs when inactive

## Quick Start

1. Sign up at console.neon.tech
2. Create your first project
3. Connect using your preferred client
```

---

## Media components

Components for embedding and displaying multimedia content.

### YoutubeIframe

Embedded YouTube video player.

```mdx
<YoutubeIframe embedId="IcoOpnAcO1Y" />
```

**Live preview:**

[Watch on YouTube](https://youtube.com/watch?v=IcoOpnAcO1Y)

---

### Video

Native video player component.

```mdx
<Video
  sources={[{ src: '/videos/pages/doc/neon-mcp.mp4', type: 'video/mp4' }]}
  width={960}
  height={1080}
/>
```

**Live preview:**

---

## Specialized shared components

Status indicators for features in different stages of development and release.

### Feature Announcements

Status indicators for features in different stages:

```mdx
<ComingSoon />
<PrivatePreview />
<PrivatePreviewEnquire />
<EarlyAccess />
<PublicPreview />
<FeatureBeta />
<LRBeta />
<NewPricing />
<LRNotice />
<MigrationAssistant />
```

**Live preview:**

**Coming Soon: Feature coming soon**

This feature is currently available to members in our Early Access Program. Sign up [here](https://console.neon.tech/app/settings/early-access) or from your user profile settings in the [Neon Console](https://console.neon.tech/app/settings/early-access).

**Coming Soon: Private Preview**

This feature is currently accessible in Private Preview only.

**Coming Soon: Early Access**

This feature is currently available in Early Access for invited users. Want to try it out? Message us from the [Console](https://console.neon.tech/app/projects?modal=feedback) or on [Discord](https://discord.gg/92vNTzKDGp) and we'll send you an invite.

**Coming Soon: Early Access**

This feature is available for members of our [Early Access Program](https://neon.com/docs/introduction/early-access). Read more about joining up [here](https://neon.com/docs/introduction/early-access).

**Note: Public Preview**

This feature is in Public Preview. Please provide [Feedback](https://console.neon.tech/app/projects?modal=feedback) from the Neon Console or by connecting with us on [Discord](https://discord.gg/92vNTzKDGp).

**Note: Beta**

This feature is in Beta. Please give us [Feedback](https://console.neon.tech/app/projects?modal=feedback) from the Neon Console or by connecting with us on [Discord](https://discord.gg/92vNTzKDGp).

**Note: Beta**

Replicating data to Neon, where Neon is configured as a subscriber in a Postgres logical replication setup, is currently in Beta. We welcome your feedback to help improve this feature. You can provide feedback via the [Feedback](https://console.neon.tech/app/projects?modal=feedback) form in the Neon Console or by reaching out to us on [Discord](https://discord.gg/92vNTzKDGp).

**Coming Soon: New pricing plans**

On February 19th, 2024, Neon will launch new, simplified pricing plans. You can see the [details of all the plans here](https://neon.com/2024-plan-updates).

**Important: Enrollment Pause for Logical Replication Beta**

We have temporarily paused new enrollments in our Logical Replication Beta program. This pause is aimed at evaluating the feature's performance and incorporating feedback from our early adopters. Please stay tuned for updates, and thank you for your interest. We plan to reopen enrollment again soon.

**Note: New feature**

If you are looking to migrate your database to Neon, you may want to try our new **Migration Assistant**, which can help. Read the [guide](https://neon.com/docs/import/migration-assistant) to learn more.

---

### EarlyAccessProps

Status indicator for early access features with custom feature name.

```mdx
<EarlyAccessProps feature_name="My Feature" />
```

**Live preview:**

**Coming Soon: Early Access**

**My Feature** is available for members of our Early Access Program.<br/>
[Sign up](https://neon.com/docs/introduction/early-access) and help shape the future of Neon.

---

## SDK components

Auto-generated components specifically for SDK documentation. These load content from [shared templates](https://neon.com/docs/community/component-guide#common-shared-components).

### Getting Started

```mdx
<GetStarted sdkName="Next.js" />
```

### SDK Type Components

```mdx
<SdkUser sdkName="React" />
<SdkProject sdkName="Node.js" />
<SdkUseUser sdkName="Vue" />
```

**Note**: These SDK components require corresponding files in the `content/docs/shared-content/` directory and proper configuration in `sharedMdxComponents`.

---

## Utility components

Specialized UI components for specific use cases.

### RequestForm

Contact or request submission form.

There are types (`extension` and `regions`) defined in `src/components/shared/request-form/data.js`. To define a new type, define a new config in `data.js` and register it in `DATA` there.

```mdx
<RequestForm type="extension" />
```

**Live preview:**

> **Request a new extension**
>
> Looking for a specific Postgres extension in Neon? Submit a request through [Neon support](https://neon.com/docs/introduction/support) or let us know in the [Neon Discord](https://discord.gg/92vNTzKDGp).

---

### ChatOptions

Chat interface options component with a specific use case in the sidebar navigation.

**Important:** This component is designed for internal navigation use only and should not be used in regular documentation content.

---

## Specialized components

Complex components for specialized functionality and workflows.

### AgentSkillsTip

Shared tip admonition that links to the Agent Skills page with a customizable topic.

```mdx
<AgentSkillsTip skill_topic="the Neon Serverless Driver, general connection advice," />
```

**Live preview:**

**Tip: AI assistant support**

Neon's [Agent Skills](https://neon.com/docs/ai/agent-skills) give AI coding assistants context about the Neon Serverless Driver, general connection advice, and other Neon features. Install them for more accurate code suggestions.

---

### MCPTools

Model Context Protocol tools integration component.

```mdx
<MCPTools />
```

**Live preview:**

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

---

## Community components

Components for community engagement and interaction.

### CommunityBanner

Community engagement banner for promoting community participation.

```mdx
<CommunityBanner buttonText="Join Discord" buttonUrl="https://discord.gg/92vNTzKDGp" logo="discord">
  Connect with the Neon community!
</CommunityBanner>
```

**Live preview:**

Connect with the Neon community!: [Join Discord](https://discord.gg/92vNTzKDGp)

---

## Usage guidelines

### When to use specialized components

- **ExternalCode**: When you need to embed code from external repositories or sources
- **Media Components**: For video tutorials, demos, or multimedia content
- **Specialized Shared Components**: For feature announcements and status updates
- **SDK Components**: For auto-generated SDK documentation
- **Utility Components**: For specialized forms and navigation elements
- **Specialized Components**: For AI-powered features and complex workflows
- **Community Components**: For community engagement and participation

### Best practices

- **Use sparingly**: These components are specialized and should be used only when appropriate
- **Test thoroughly**: Specialized components may have specific requirements or dependencies
- **Document usage**: When using specialized components, document the specific use case
- **Consider alternatives**: Always consider if a simpler component would work better
- **Follow patterns**: Use established patterns for similar functionality

### Component dependencies

Some specialized components have specific dependencies:

- **SDK Components**: Require shared content files and proper configuration
- **RequestForm**: Requires specific type configurations in the data files
- **Media Components**: May require specific video formats or hosting
- **Specialized Components**: May have AI or external service dependencies

---

## Component summary

This guide covers specialized MDX components used in specific scenarios. Each component includes:

- **MDX syntax**: Copy-paste ready code examples
- **Live rendering**: See exactly how components appear
- **Usage guidelines**: When and how to use each component
- **Dependencies**: Requirements and configurations needed

### Component categories

| **Category**                                                                                                             | **Components**                                                                                                                                                                                   | **Use Case**                          |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------- |
| **[Code Display](https://neon.com/docs/community/component-specialized#code-display)**                                   | [ExternalCode](https://neon.com/docs/community/component-specialized#externalcode)                                                                                                               | External code embedding               |
| **[Media Components](https://neon.com/docs/community/component-specialized#media-components)**                           | [YoutubeIframe](https://neon.com/docs/community/component-specialized#youtubeiframe), [Video](https://neon.com/docs/community/component-specialized#video)                                       | Multimedia content                    |
| **[Specialized Shared Components](https://neon.com/docs/community/component-specialized#specialized-shared-components)** | [Feature Announcements](https://neon.com/docs/community/component-specialized#feature-announcements), [EarlyAccessProps](https://neon.com/docs/community/component-specialized#earlyaccessprops) | Feature status indicators             |
| **[SDK Components](https://neon.com/docs/community/component-specialized#sdk-components)**                               | [Getting Started](https://neon.com/docs/community/component-specialized#getting-started), [SDK Type Components](https://neon.com/docs/community/component-specialized#sdk-type-components)       | Auto-generated SDK documentation      |
| **[Utility Components](https://neon.com/docs/community/component-specialized#utility-components)**                       | [RequestForm](https://neon.com/docs/community/component-specialized#requestform), [ChatOptions](https://neon.com/docs/community/component-specialized#chatoptions)                               | Specialized UI elements               |
| **[Specialized Components](https://neon.com/docs/community/component-specialized#specialized-components)**               | [AgentSkillsTip](https://neon.com/docs/community/component-specialized#agentskillstip), [MCPTools](https://neon.com/docs/community/component-specialized#mcptools)                               | Complex and specialized functionality |
| **[Community Components](https://neon.com/docs/community/component-specialized#community-components)**                   | [CommunityBanner](https://neon.com/docs/community/component-specialized#communitybanner)                                                                                                         | Community engagement                  |

For commonly used components, see the [Component Guide](https://neon.com/docs/community/component-guide).

---

## Related docs (Community)

- [Community hub](https://neon.com/docs/community/community-intro)
- [Docs contribution guide](https://neon.com/docs/community/contribution-guide)
- [Using Mermaid diagrams](https://neon.com/docs/community/mermaid-diagrams)
- [Component guide](https://neon.com/docs/community/component-guide)
- [Component icon guide](https://neon.com/docs/community/component-icon-guide)
- [Component architecture](https://neon.com/docs/community/component-architecture)
- [AI tools for documentation](https://neon.com/docs/community/ai-tools)
- [Using docs as Markdown (LLMs)](https://neon.com/docs/community/llms-markdown-guide)
