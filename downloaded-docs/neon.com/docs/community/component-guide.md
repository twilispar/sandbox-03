> This page location: Community > Component guide
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Step-by-step guide for using common MDX components in Neon documentation, including best practices, syntax, and prop usage for effective content organization and presentation.

# Component guide

Most commonly used components for documentation writers

A practical guide for the most commonly used MDX components in Neon documentation. This guide focuses on components you'll use most frequently when writing documentation.

**What you will learn:**

- How to use common MDX components in Neon docs
- How to choose between different components
- Best practices
- Proper syntax and prop usage

**Related topics**

- [Component Specialized Guide](https://neon.com/docs/community/component-specialized)
- [Component Icon Guide](https://neon.com/docs/community/component-icon-guide)
- [Component Architecture](https://neon.com/docs/community/component-architecture)
- [Documentation Contribution Guide](https://neon.com/docs/community/contribution-guide)

## Quick navigation

- [Essential components](https://neon.com/docs/community/component-guide#essential-components) - Most commonly used
- [Tabbed content](https://neon.com/docs/community/component-guide#tabbed-content) - CodeTabs and Tabs for organized content
- [Content organization](https://neon.com/docs/community/component-guide#content-organization) - Structure and navigation components
- [Interactive elements](https://neon.com/docs/community/component-guide#interactive-elements) - UI elements and forms
- [Common shared components](https://neon.com/docs/community/component-guide#common-shared-components) - Reusable content
- [External content](https://neon.com/docs/community/component-guide#external-content) - Embedding external code and video

---

## Essential components

These are the most frequently used components in Neon docs.

### Admonition

Callouts for notes, warnings, and tips. There are six types available: `note` (default), `important`, `tip`, `info`, `warning`, `comingSoon`.

```mdx

<Admonition type="warning" title="Important">
Critical information requiring immediate attention.
</Admonition>
```

**Live preview:**

**Warning: Important**

Critical information requiring immediate attention.

**All Admonition types:**

**Note: Note**

Highlights information that users should take into account.

**Important:** Crucial information necessary for users to succeed.

**Tip: Pro tip**

Optional information to help a user be more successful.

**Info:** Information that helps users understand things better.

**Warning:** Critical content demanding immediate user attention due to potential risks.

**Coming Soon:** Information about features that are coming soon.

---

### Callout

Highlighted block for supplementary information the reader should notice, but that doesn't carry the urgency of an Admonition. Use it for tips, best practices, or "good to know" context. The default label is "Good to know".

```mdx
<Callout title="Before you start">

Make sure you have Node.js 18+ installed.

</Callout>
```

**Live preview:**

**Before you start:**

Make sure you have Node.js 18+ installed.

**Props:**

| Prop       | Type   | Default        | Description                         |
| ---------- | ------ | -------------- | ----------------------------------- |
| `children` | node   | (required)     | Content rendered inside the callout |
| `title`    | string | `Good to know` | Label displayed in the header       |

**When to use Callout vs Admonition:**

- **Callout** — supplementary context, best practices, or neutral "good to know" information.
- **Admonition** — warnings, important notices, tips with urgency, or coming-soon flags. Use when missing the information could cause user error.

---

### Steps

Numbered step-by-step instructions split by `h2` headings.

```mdx
<Steps>

## Get a Glass

Take a clean glass from the cabinet or dish rack.

## Turn on Tap

Adjust the faucet to your preferred temperature and flow rate.

## Fill and Drink

Fill the glass to desired level and enjoy your water.

</Steps>
```

**Live preview:**

## Get a Glass

Take a clean glass from the cabinet or dish rack.

## Turn on Tap

Adjust the faucet to your preferred temperature and flow rate.

## Fill and Drink

Fill the glass to desired level and enjoy your water.

---

## Tabbed content

Components for organizing content into tabs.

### CodeTabs

Multi-language code examples with tabs.

````mdx
<CodeTabs labels={["JavaScript", "Python", "Go"]}>

```javascript
const { Client } = require('pg');
const client = new Client({
  connectionString: process.env.DATABASE_URL,
});
await client.connect();
```

```python
import psycopg2
import os

conn = psycopg2.connect(os.environ["DATABASE_URL"])
cur = conn.cursor()
```

```go
import (
    "database/sql"
    _ "github.com/lib/pq"
)

db, err := sql.Open("postgres", os.Getenv("DATABASE_URL"))
```

</CodeTabs>
````

**Live preview:**

**JavaScript**

```javascript
const { Client } = require('pg');
const client = new Client({
  connectionString: process.env.DATABASE_URL,
});
await client.connect();
```

**Python**

```python
import psycopg2
import os

conn = psycopg2.connect(os.environ["DATABASE_URL"])
cur = conn.cursor()
```

**Go**

```go
import (
    "database/sql"
    _ "github.com/lib/pq"
)

db, err := sql.Open("postgres", os.Getenv("DATABASE_URL"))
```

---

### Tabs

General tabbed content (not just code). For code-specific tabs, use [CodeTabs](https://neon.com/docs/community/component-guide#codetabs) instead.

````mdx
<Tabs labels={["Console", "CLI", "API"]}>
<TabItem>
Create a database using the Neon Console by navigating to your project dashboard and clicking "Create Database".
</TabItem>
<TabItem>
Use the Neon CLI to create a database:

```bash
neon databases create --name my-database
```

</TabItem>
<TabItem>
Use the API to create a database:

```bash
curl -X POST https://console.neon.tech/api/v2/projects/my-project/databases \
  -H "Authorization: Bearer $NEON_API_KEY"
```

</TabItem>
</Tabs>
````

**Live preview:**

**Console**

Create a database using the Neon Console by navigating to your project dashboard and clicking "Create Database".

**CLI**

Use the Neon CLI to create a database:

```bash
neon databases create --name my-database
```

**API**

Use the API to create a database:

```bash
curl -X POST https://console.neon.tech/api/v2/projects/my-project/databases \
  -H "Authorization: Bearer $NEON_API_KEY"
```

---

## Content organization

Components for structuring and organizing page content.

### TechCards / DetailIconCards

Technology cards with icons, titles, and descriptions. These components use different [icon systems](https://neon.com/docs/community/component-icon-guide) - see the [comparison table](https://neon.com/docs/community/component-guide#techcards-vs-detailiconcards-vs-docslist) below to choose the right one.

#### TechCards

Standard technology cards layout using [TechCards icons](https://neon.com/docs/community/component-icon-guide#techcards-icons):

```mdx
<TechCards>
  <a
    href="/docs/guides/node"
    title="Node.js"
    description="Connect Node.js applications to Neon"
    icon="node-js"
  >
    Node.js
  </a>
  <a
    href="/docs/guides/python"
    title="Python"
    description="Connect Python applications to Neon"
    icon="python"
  >
    Python
  </a>
  <a
    href="/docs/guides/nextjs"
    title="Next.js"
    description="Build Next.js apps with Neon"
    icon="next-js"
  >
    Next.js
  </a>
</TechCards>
```

**Live preview:**

- [Node.js](https://neon.com/docs/guides/node): Connect Node.js applications to Neon
- [Python](https://neon.com/docs/guides/python): Connect Python applications to Neon
- [Next.js](https://neon.com/docs/guides/nextjs): Build Next.js apps with Neon

---

#### DetailIconCards

Alternative layout using [DetailIconCards icons](https://neon.com/docs/community/component-icon-guide#detailiconcards-icons):

```mdx
<DetailIconCards>
  <a
    href="/docs/ai/openai"
    title="OpenAI integration"
    description="Build AI features with OpenAI"
    icon="openai"
  >
    OpenAI Integration
  </a>
  <a
    href="/docs/ai/langchain"
    title="LangChain integration"
    description="Create AI workflows with LangChain"
    icon="langchain"
  >
    LangChain Integration
  </a>
  <a
    href="/docs/development"
    title="Code development"
    description="Development tools and practices"
    icon="code"
  >
    Code Development
  </a>
  <a
    href="/docs/cloud/aws"
    title="AWS integration"
    description="Deploy and scale with AWS"
    icon="aws"
  >
    AWS Integration
  </a>
</DetailIconCards>
```

**Live preview:**

- [OpenAI Integration](https://neon.com/docs/ai/openai): Build AI features with OpenAI
- [LangChain Integration](https://neon.com/docs/ai/langchain): Create AI workflows with LangChain
- [Code Development](https://neon.com/docs/development): Development tools and practices
- [AWS Integration](https://neon.com/docs/cloud/aws): Deploy and scale with AWS

---

> DetailIconCards uses a different [icon system](https://neon.com/docs/community/component-icon-guide#detailiconcards-icons) than [TechCards](https://neon.com/docs/community/component-icon-guide#techcards-icons), which is why different icons are available._

#### TechCards vs DetailIconCards vs DocsList

Quick comparison to help you choose the right component:

| Component                                                                              | Use For                        | Icon System                                                                                             | Layout      |
| -------------------------------------------------------------------------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------- | ----------- |
| **[TechCards](https://neon.com/docs/community/component-guide#techcards)**             | Technology/framework showcases | [Technology logos](https://neon.com/docs/community/component-icon-guide#techcards-icons) (colorful)     | Card grid   |
| **[DetailIconCards](https://neon.com/docs/community/component-guide#detailiconcards)** | Feature/service showcases      | [Detail icons](https://neon.com/docs/community/component-icon-guide#detailiconcards-icons) (monochrome) | Card grid   |
| **[DocsList](https://neon.com/docs/community/component-guide#docslist)**               | Documentation links            | Checkbox (default), docs, or repo icon                                                                  | Simple list |

### DefinitionList

Accessible term/definition lists for defining technical terms and concepts.

```mdx
<DefinitionList>

Database URL
: Connection string for your Neon database
: Format: `postgresql://user:password@host:port/database`

Connection Pool
: A cache of database connections
: Improves performance by reusing connections

Branch
: An isolated copy of your database
: Used for development and testing

</DefinitionList>
```

**Live preview:**

Database URL
: Connection string for your Neon database
: Format: `postgresql://user:password@host:port/database`

Connection Pool
: A cache of database connections
: Improves performance by reusing connections

Branch
: An isolated copy of your database
: Used for development and testing

---

### DocsList

Simple, clean lists for documentation links with optional theming. DocsList provides a lightweight alternative to card-based components for presenting navigation links or content summaries.

**Props:**

- `title` (string) - Optional title for the list section
- `theme` (string) - Visual theme: `"docs"` (document icon), `"repo"` (repository icon), or default (checkbox icon)

**Default Theme (Checkbox Icon):**

**MDX Code:**

```mdx
<DocsList title="Related documentation">
  <a href="/docs/guides/node">Node.js Connection Guide</a>
  <a href="/docs/guides/python">Python Connection Guide</a>
  <a href="/docs/api-reference">API Reference</a>
  <a href="/docs/cli">CLI Documentation</a>
</DocsList>
```

**Live preview:**

**Related documentation**

- [Node.js Connection Guide](https://neon.com/docs/guides/node)
- [Python Connection Guide](https://neon.com/docs/guides/python)
- [API Reference](https://neon.com/docs/api-reference)
- [CLI Documentation](https://neon.com/docs/cli)

---

### InfoBlock

InfoBlock creates a multi-column layout for organizing related content sections. Use it for "at-a-glance" summaries at the top of documentation pages, combining learning objectives with related resources. Use two columns.

**Key Features:**

- Commonly paired with DocsList for structured content presentation
- Ideal for page introductions and overview sections

**Basic Two-Column Layout:**

```mdx
<InfoBlock>
<DocsList title="What you will learn:">
<p>How to view and modify data in the console</p>
<p>Create an isolated database copy per developer</p>
<p>Reset your branch to production when ready to start new work</p>
</DocsList>

<DocsList title="Related topics" theme="docs">
<a href="/docs/introduction/branching">About branching</a>
<a href="/docs/get-started/workflow-primer">Branching workflows</a>
<a href="/docs/get-started/connect-neon">Connect Neon to your stack</a>
</DocsList>
</InfoBlock>
```

**Renders as:**

**What you will learn:**

- How to view and modify data in the console
- Create an isolated database copy per developer
- Reset your branch to production when ready to start new work

**Related topics**

- [About branching](https://neon.com/docs/introduction/branching)
- [Branching workflows](https://neon.com/docs/get-started/workflow-primer)
- [Connect Neon to your stack](https://neon.com/docs/get-started/connect-neon)

---

### TwoColumnLayout

Two-column layout for tutorials and reference documentation. Use `TwoColumnLayout.Step` for numbered tutorial steps, `TwoColumnLayout.Item` for reference items.

Add `layout: wide` to the page frontmatter when using this component, to hide the right sidebar and give the layout more room.

````mdx
<TwoColumnLayout>

<TwoColumnLayout.Step title="Install dependencies">
<TwoColumnLayout.Block>

Install the required packages.

</TwoColumnLayout.Block>
<TwoColumnLayout.Block label="Terminal">

```bash
npm install @neondatabase/neon-js
```

</TwoColumnLayout.Block>
</TwoColumnLayout.Step>

</TwoColumnLayout>
````

**Subcomponents:**

| Subcomponent             | Props                   | Purpose                                    |
| ------------------------ | ----------------------- | ------------------------------------------ |
| `TwoColumnLayout.Step`   | `title`                 | Numbered step for tutorials                |
| `TwoColumnLayout.Item`   | `title`, `method`, `id` | Reference item                             |
| `TwoColumnLayout.Block`  | `label` (optional)      | Content block within a step or item        |
| `TwoColumnLayout.Footer` | —                       | Full-width content at the bottom of a step |

See [Neon Auth with Next.js](https://neon.com/docs/auth/quick-start/nextjs-api-only) for a live example.

---

### FeatureList

Visual list of features, split by `##` and `###` headings. Supports an optional `icons` prop.

```mdx
<FeatureList>

### Instant provisioning

Create databases in seconds.

### Autoscaling

Scale compute up and down automatically.

</FeatureList>
```

To add icons, pass an array of icon names (see `src/components/shared/feature-list/icon/icon.jsx` for available icons):

```mdx
<FeatureList icons={['agent', 'speedometer']}>
```

---

## Interactive elements

Components for user engagement and interaction.

### CheckList

Interactive checklists for setup guides and tutorials. CheckList uses CheckItem components internally.

```mdx
<CheckList title="Setup checklist">
  <CheckItem title="Create Neon account" href="#signup">
    Sign up for a free Neon account at console.neon.tech
  </CheckItem>
  <CheckItem title="Install dependencies" href="#install">
    Install the required packages for your project
  </CheckItem>
  <CheckItem title="Configure environment" href="#config">
    Set up your database connection string
  </CheckItem>
  <CheckItem title="Test connection" href="#test">
    Verify your application can connect to Neon
  </CheckItem>
</CheckList>
```

**Live preview:**

## Setup checklist

- [ ] [Create Neon account](https://neon.com/docs/community/component-guide#signup)
    Sign up for a free Neon account at console.neon.tech
- [ ] [Install dependencies](https://neon.com/docs/community/component-guide#install)
    Install the required packages for your project
- [ ] [Configure environment](https://neon.com/docs/community/component-guide#config)
    Set up your database connection string
- [ ] [Test connection](https://neon.com/docs/community/component-guide#test)
    Verify your application can connect to Neon

---

### CheckItem

Individual checklist items used within CheckList components.

```mdx
<CheckItem title="Task name" href="#anchor">
  Description of the task or requirement
</CheckItem>
```

---

**Usage Notes:**

- Always used within a `<CheckList>` component
- `title` prop is required
- `href` prop is optional for anchor linking
- Content is the description text

### CTA (Call to Action)

Prominent call-to-action buttons for important actions.

```mdx
<CTA
  title="Try Neon free"
  description="Start building with serverless Postgres today. No credit card required."
  buttonText="Sign Up"
  buttonUrl="https://console.neon.tech/signup"
/>
```

**Live preview:**

> **Try Neon free**
>
> Start building with serverless Postgres today. No credit card required.
>
> [Sign Up](https://console.neon.tech/signup)

---

### CopyPrompt

Displays a copyable LLM prompt from a file. Use when providing a pre-built prompt to help users get started faster. Prompt files go in `public/prompts/`.

```mdx
<CopyPrompt
  src="/prompts/my-prompt.md"
  displayText="Use this pre-built prompt to get started faster."
  buttonText="Copy prompt"
/>
```

**Props:**

| Prop          | Type   | Default                                            | Description                                  |
| ------------- | ------ | -------------------------------------------------- | -------------------------------------------- |
| `src`         | string | (required)                                         | Path to the prompt file in `public/prompts/` |
| `displayText` | string | `Use this pre-built prompt to get started faster.` | CTA text shown to the left                   |
| `buttonText`  | string | `Copy prompt`                                      | Button label                                 |

---

### NeedHelp

Support widget for getting assistance.

```mdx
<NeedHelp />
```

**Live preview:**

---

---

## Common shared components

Reusable content components that load from shared templates.

### LinkAPIKey

Link to API key management in the console.

```mdx
<LinkAPIKey />
```

**Live preview:**

**Note:** To learn more about the types of API keys you can create — personal, organization, or project-scoped — see [Manage API Keys](https://neon.com/docs/manage/api-keys).

---

### FeatureBetaProps

Status indicator for beta features with custom feature name.

```mdx
<FeatureBetaProps feature_name="OpenTelemetry integration" />
```

**Live preview:**

**Note: Beta**

The **OpenTelemetry integration** is in Beta. Share your feedback on [Discord](https://discord.gg/92vNTzKDGp) or via the [Neon Console](https://console.neon.tech/app/projects?modal=feedback).

---

---

## External content

Components for embedding content from outside the repo.

### ExternalCode

Embed code from an external URL with syntax highlighting. Always use raw GitHub URLs.

```mdx
<ExternalCode
  url="https://raw.githubusercontent.com/neondatabase/neon/main/README.md"
  language="markdown"
/>
```

**Props:**

| Prop              | Type    | Default               | Description                      |
| ----------------- | ------- | --------------------- | -------------------------------- |
| `url`             | string  | (required)            | Raw URL to the file              |
| `language`        | string  | (auto from extension) | Language for syntax highlighting |
| `showLineNumbers` | boolean | false                 | Show line numbers                |
| `shouldWrap`      | boolean | false                 | Enable code wrapping             |

### YoutubeIframe

Embeds a YouTube video player.

```mdx
<YoutubeIframe embedId="IcoOpnAcO1Y" />
```

Pass the video ID from the YouTube URL (the part after `v=`).

---

## Best practices

### Component selection

- **[Admonition](https://neon.com/docs/community/component-guide#admonition)** for urgent callouts and warnings
- **[Callout](https://neon.com/docs/community/component-guide#callout)** for supplementary "good to know" context
- **[Steps](https://neon.com/docs/community/component-guide#steps)** for sequential instructions
- **[CodeTabs](https://neon.com/docs/community/component-guide#codetabs)** for multi-language examples
- **[TechCards](https://neon.com/docs/community/component-guide#techcards)** for technology showcases
- **[TwoColumnLayout](https://neon.com/docs/community/component-guide#twocolumnlayout)** for tutorials and reference docs requiring a two-column layout
- **[FeatureList](https://neon.com/docs/community/component-guide#featurelist)** for listing product features visually
- **[CheckList](https://neon.com/docs/community/component-guide#checklist)** for setup guides
- **[CopyPrompt](https://neon.com/docs/community/component-guide#copyprompt)** for providing a copyable LLM prompt
- **[InfoBlock](https://neon.com/docs/community/component-guide#infoblock)** for page introductions with multiple content sections
- **[DocsList](https://neon.com/docs/community/component-guide#docslist)** for simple navigation lists with theming options

### Content organization tips

- Use [InfoBlock](https://neon.com/docs/community/component-guide#infoblock) at the top of pages to provide quick orientation
- Add `layout: wide` to frontmatter when using [TwoColumnLayout](https://neon.com/docs/community/component-guide#twocolumnlayout)
- For [TechCards](https://neon.com/docs/community/component-guide#techcards), always check that the SVG file exists in `/public/images/technology-logos/`
- For [DetailIconCards](https://neon.com/docs/community/component-guide#detailiconcards), use only icon names that are mapped in the component code
- Choose [DocsList](https://neon.com/docs/community/component-guide#docslist) themes based on content type: default for tasks, `docs` for documentation, `repo` for code
- Use the [comparison table](https://neon.com/docs/community/component-guide#techcards-vs-detailiconcards-vs-docslist) to choose between [TechCards](https://neon.com/docs/community/component-guide#techcards), [DetailIconCards](https://neon.com/docs/community/component-guide#detailiconcards), and [DocsList](https://neon.com/docs/community/component-guide#docslist)

---

## Component summary

This guide covers the most commonly used MDX components in Neon documentation. Each component includes:

- **MDX syntax**: Copy-paste ready code examples
- **Live rendering**: See exactly how components appear
- **Props documentation**: Available parameters and options
- **Best practices**: When and how to use each component

### Component categories

| **Category**                                                                                             | **Components**                                                                                                                                                                                                                                                                                                                                                                                                         | **Use Case**                           |
| -------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| **[Essential](https://neon.com/docs/community/component-guide#essential-components)**                    | [Admonition](https://neon.com/docs/community/component-guide#admonition), [Steps](https://neon.com/docs/community/component-guide#steps)                                                                                                                                                                                                                                                                               | Most commonly used components          |
| **[Tabbed Content](https://neon.com/docs/community/component-guide#tabbed-content)**                     | [CodeTabs](https://neon.com/docs/community/component-guide#codetabs), [Tabs](https://neon.com/docs/community/component-guide#tabs)                                                                                                                                                                                                                                                                                     | Organizing content into tabs           |
| **[Content organization](https://neon.com/docs/community/component-guide#content-organization)**         | [TechCards](https://neon.com/docs/community/component-guide#techcards--detailiconcards), [DetailIconCards](https://neon.com/docs/community/component-guide#techcards--detailiconcards), [DefinitionList](https://neon.com/docs/community/component-guide#definitionlist), [DocsList](https://neon.com/docs/community/component-guide#docslist), [InfoBlock](https://neon.com/docs/community/component-guide#infoblock) | Structure and navigation               |
| **[Interactive elements](https://neon.com/docs/community/component-guide#interactive-elements)**         | [CheckList](https://neon.com/docs/community/component-guide#checklist), [CheckItem](https://neon.com/docs/community/component-guide#checkitem), [CTA](https://neon.com/docs/community/component-guide#cta-call-to-action), [NeedHelp](https://neon.com/docs/community/component-guide#needhelp)                                                                                                                        | User engagement and interaction        |
| **[Common shared components](https://neon.com/docs/community/component-guide#common-shared-components)** | [LinkAPIKey](https://neon.com/docs/community/component-guide#linkapikey), [FeatureBetaProps](https://neon.com/docs/community/component-guide#featurebetaprops)                                                                                                                                                                                                                                                         | Reusable content and status indicators |

For specialized components and specific use cases, see the [Component Specialized Guide](https://neon.com/docs/community/component-specialized).

---

## Related docs (Community)

- [Community hub](https://neon.com/docs/community/community-intro)
- [Docs contribution guide](https://neon.com/docs/community/contribution-guide)
- [Using Mermaid diagrams](https://neon.com/docs/community/mermaid-diagrams)
- [Component specialized guide](https://neon.com/docs/community/component-specialized)
- [Component icon guide](https://neon.com/docs/community/component-icon-guide)
- [Component architecture](https://neon.com/docs/community/component-architecture)
- [AI tools for documentation](https://neon.com/docs/community/ai-tools)
- [Using docs as Markdown (LLMs)](https://neon.com/docs/community/llms-markdown-guide)
