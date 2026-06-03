> This page location: Community > Component icon guide
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the selection and implementation of various icon systems in Neon, detailing available icons, usage guidelines, and best practices for effective integration in components.

# Component icon guide

Complete reference for all icon systems and usage

A comprehensive guide to all icon systems used in Neon documentation. This guide helps you understand which icon system to use and how to implement them correctly.

**What you will learn:**

- How to choose the right icon system for your use case
- Which icons are available in each system
- How to implement icons correctly in components
- Best practices for icon usage and troubleshooting

**Related topics**

- [Component Guide](https://neon.com/docs/community/component-guide)
- [Component Specialized Guide](https://neon.com/docs/community/component-specialized)
- [Component Architecture](https://neon.com/docs/community/component-architecture)
- [Documentation Contribution Guide](https://neon.com/docs/community/contribution-guide)

## Quick navigation

- [Icon system overview](https://neon.com/docs/community/component-icon-guide#icon-system-overview) - Understanding the different icon systems
- [TechCards Icons](https://neon.com/docs/community/component-icon-guide#techcards-icons) - Technology logos and frameworks
- [DetailIconCards Icons](https://neon.com/docs/community/component-icon-guide#detailiconcards-icons) - Feature and service icons
- [Icon usage guidelines](https://neon.com/docs/community/component-icon-guide#icon-usage-guidelines) - Best practices and conventions
- [Icon Decision Tree](https://neon.com/docs/community/component-icon-guide#icon-decision-tree) - Which icon system to use when

---

## Icon system overview

Neon documentation uses multiple icon systems for different components. Understanding which system to use is crucial for proper implementation.

### Available icon systems

1. **TechCards Icons** - Colorful technology logos and framework icons
2. **DetailIconCards Icons** - Monochrome feature and service icons
3. **DocsList Icons** - Simple navigation and action icons

### Component compatibility

| Component           | Icon System           | Use Case                       |
| ------------------- | --------------------- | ------------------------------ |
| **TechCards**       | TechCards Icons       | Technology/framework showcases |
| **DetailIconCards** | DetailIconCards Icons | Feature/service showcases      |
| **DocsList**        | Built-in themes       | Documentation links            |

### Usage example

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
</DetailIconCards>
```

_For technical implementation details, see the [Icon Systems section](https://neon.com/docs/community/component-architecture#icon-systems) in the Component Architecture guide._

---

## Icon usage guidelines

### Choosing the right icon system

- **TechCards**: Use for technology and framework showcases
- **DetailIconCards**: Use for feature and service showcases
- **DocsList**: Use built-in themes for documentation links

### Icon naming conventions

- **TechCards**: Use kebab-case (for example, `node-js`, `next-js`)
- **DetailIconCards**: Use camelCase or kebab-case as defined in the mapping
- **File Requirements**: Ensure SVG files exist in the correct directories

### Best practices

1. **Test your icons**: Always verify that your chosen icon works in the target component
2. **Check both systems**: If an icon doesn't work in one system, try the other
3. **Use descriptive names**: Choose icon names that clearly represent the technology or feature
4. **Document your choices**: Keep track of which icons work in which components
5. **Fallback gracefully**: Provide alternative text or descriptions when icons aren't available

---

### Component selection guide

| Use Case            | Component       | Icon System           | Example                |
| ------------------- | --------------- | --------------------- | ---------------------- |
| Technology showcase | TechCards       | TechCards Icons       | Node.js, React, Python |
| Feature showcase    | DetailIconCards | DetailIconCards Icons | OpenAI, AWS, Database  |
| Documentation links | DocsList        | Built-in themes       | Guides, API docs       |
| Simple navigation   | DocsList        | Built-in themes       | Related topics         |

---

## Troubleshooting

### Common issues

1. **Icon not displaying**: Check that the icon name exists in the correct system
2. **Wrong icon system**: Verify you're using the right component for your icon
3. **Missing files**: Ensure SVG files exist in the correct directories
4. **Case sensitivity**: Icon names are case-sensitive

### Testing icons

To test if an icon works:

1. **TechCards**: Check if `{icon}.svg` exists in `/public/images/technology-logos/`
2. **DetailIconCards**: Check if the icon is mapped in the component code
3. **DocsList**: Use the built-in themes (default, docs, repo)

### Getting help

- Review the [Component Architecture](https://neon.com/docs/community/component-architecture) for technical details
- Test icons in a development environment before using in production

---

## Complete icon showcase

### TechCards Icons (Technology Logos)

#### Programming Languages & Frameworks

- [Bun](https://neon.com/docs/community/component-icon-guide#): JavaScript runtime (icon: bun)
- [Express](https://neon.com/docs/community/component-icon-guide#): Node.js web framework (icon: express)
- [Hasura](https://neon.com/docs/community/component-icon-guide#): GraphQL API engine (icon: hasura)
- [Hono](https://neon.com/docs/community/component-icon-guide#): Web framework (icon: hono)
- [Java](https://neon.com/docs/community/component-icon-guide#): Programming language (icon: java)
- [JavaScript](https://neon.com/docs/community/component-icon-guide#): Programming language (icon: javascript)
- [Laravel](https://neon.com/docs/community/component-icon-guide#): PHP framework (icon: laravel)
- [Nest.js](https://neon.com/docs/community/component-icon-guide#): Node.js framework (icon: nest-js)
- [Next.js](https://neon.com/docs/community/component-icon-guide#): React framework (icon: next-js)
- [Node.js](https://neon.com/docs/community/component-icon-guide#): JavaScript runtime (icon: node-js)
- [Nuxt](https://neon.com/docs/community/component-icon-guide#): Vue framework (icon: nuxt)
- [Phoenix](https://neon.com/docs/community/component-icon-guide#): Elixir framework (icon: phoenix)
- [Python](https://neon.com/docs/community/component-icon-guide#): Programming language (icon: python)
- [Quarkus](https://neon.com/docs/community/component-icon-guide#): Java framework (icon: quarkus)
- [Rails](https://neon.com/docs/community/component-icon-guide#): Ruby framework (icon: rails)
- [React](https://neon.com/docs/community/component-icon-guide#): JavaScript library (icon: react)
- [Redwood SDK](https://neon.com/docs/community/component-icon-guide#): Full-stack framework (icon: redwoodsdk)
- [Reflex](https://neon.com/docs/community/component-icon-guide#): Python framework (icon: reflex)
- [Remix](https://neon.com/docs/community/component-icon-guide#): React framework (icon: remix)
- [Ruby](https://neon.com/docs/community/component-icon-guide#): Programming language (icon: ruby)
- [Rust](https://neon.com/docs/community/component-icon-guide#): Programming language (icon: rust)
- [Solid](https://neon.com/docs/community/component-icon-guide#): JavaScript library (icon: solid)
- [Svelte](https://neon.com/docs/community/component-icon-guide#): JavaScript framework (icon: svelte)
- [Symfony](https://neon.com/docs/community/component-icon-guide#): PHP framework (icon: symfony)
- [Vue](https://neon.com/docs/community/component-icon-guide#): JavaScript framework (icon: vue)

#### Cloud & Infrastructure

- [AWS S3 Bucket](https://neon.com/docs/community/component-icon-guide#): Cloud storage service (icon: aws-s3-bucket)
- [Backblaze](https://neon.com/docs/community/component-icon-guide#): Cloud storage and backup (icon: backblaze)
- [Heroku](https://neon.com/docs/community/component-icon-guide#): Cloud platform (icon: heroku)
- [Koyeb](https://neon.com/docs/community/component-icon-guide#): Cloud platform (icon: koyeb)
- [Netlify](https://neon.com/docs/community/component-icon-guide#): Web hosting platform (icon: netlify)
- [Railway](https://neon.com/docs/community/component-icon-guide#): Deployment platform (icon: railway)
- [Render](https://neon.com/docs/community/component-icon-guide#): Cloud platform (icon: render)
- [Vercel](https://neon.com/docs/community/component-icon-guide#): Deployment platform (icon: vercel)

#### Databases & Data

- [Convex](https://neon.com/docs/community/component-icon-guide#): Backend platform (icon: convex)
- [Entity](https://neon.com/docs/community/component-icon-guide#): Data modeling tool (icon: entity)
- [Estuary](https://neon.com/docs/community/component-icon-guide#): Data streaming platform (icon: estuary)
- [Exograph](https://neon.com/docs/community/component-icon-guide#): GraphQL framework (icon: exograph)
- [Ferret](https://neon.com/docs/community/component-icon-guide#): Data processing tool (icon: ferret)
- [Kafka](https://neon.com/docs/community/component-icon-guide#): Stream processing platform (icon: kafka)
- [Knex](https://neon.com/docs/community/component-icon-guide#): SQL query builder (icon: knex)
- [Liquibase](https://neon.com/docs/community/component-icon-guide#): Database migration tool (icon: liquibase)
- [Materialize](https://neon.com/docs/community/component-icon-guide#): Streaming database (icon: materialize)
- [Neon](https://neon.com/docs/community/component-icon-guide#): Serverless Postgres (icon: neon)
- [Outerbase](https://neon.com/docs/community/component-icon-guide#): Database interface (icon: outerbase)
- [Draxlr](https://neon.com/docs/community/component-icon-guide#): Database interface (icon: draxlr)
- [Polyscale](https://neon.com/docs/community/component-icon-guide#): Database proxy (icon: polyscale)
- [PostgreSQL](https://neon.com/docs/community/component-icon-guide#): Database system (icon: postgresql)
- [Prisma](https://neon.com/docs/community/component-icon-guide#): ORM for Node.js (icon: prisma)
- [Sequelize](https://neon.com/docs/community/component-icon-guide#): ORM for Node.js (icon: sequelize)
- [Sequin](https://neon.com/docs/community/component-icon-guide#): Data synchronization (icon: sequin)
- [Snowflake](https://neon.com/docs/community/component-icon-guide#): Data warehouse (icon: snowflake)
- [SQLAlchemy](https://neon.com/docs/community/component-icon-guide#): Python ORM (icon: sqlalchemy)
- [StepZen](https://neon.com/docs/community/component-icon-guide#): GraphQL platform (icon: stepzen)
- [Supabase](https://neon.com/docs/community/component-icon-guide#): Backend platform (icon: supabase)
- [Tortoise ORM](https://neon.com/docs/community/component-icon-guide#): Python ORM (icon: tortoise-orm)
- [TypeORM](https://neon.com/docs/community/component-icon-guide#): TypeScript ORM (icon: typeorm)
- [Drizzle](https://neon.com/docs/community/component-icon-guide#): TypeScript ORM (icon: drizzle)

#### Services & APIs

- [Cloudinary](https://neon.com/docs/community/component-icon-guide#): Cloud media management (icon: cloudinary)
- [ImageKit](https://neon.com/docs/community/component-icon-guide#): Image optimization (icon: imagekit)
- [Inngest](https://neon.com/docs/community/component-icon-guide#): Background job platform (icon: inngest)
- [Micronaut](https://neon.com/docs/community/component-icon-guide#): Java framework (icon: micronaut)
- [Neo Tax](https://neon.com/docs/community/component-icon-guide#): Tax calculation service (icon: neo-tax)
- [OAuth](https://neon.com/docs/community/component-icon-guide#): Authentication protocol (icon: oauth)
- [Okta](https://neon.com/docs/community/component-icon-guide#): Identity platform (icon: okta)
- [Uploadcare](https://neon.com/docs/community/component-icon-guide#): File handling service (icon: uploadcare)
- [WunderGraph](https://neon.com/docs/community/component-icon-guide#): API development platform (icon: wundergraph)

### DetailIconCards Icons (Feature Icons)

#### AI & Machine Learning

- [LangChain](https://neon.com/docs/community/component-icon-guide#): AI framework (icon: langchain)
- [LlamaIndex](https://neon.com/docs/community/component-icon-guide#): Data framework for LLMs (icon: llamaindex)
- [Ollama](https://neon.com/docs/community/component-icon-guide#): Local LLM framework (icon: ollama)
- [OpenAI](https://neon.com/docs/community/component-icon-guide#): AI platform (icon: openai)

#### Development Tools

- [Atom](https://neon.com/docs/community/component-icon-guide#): Code editor (icon: atom)
- [Binary Code](https://neon.com/docs/community/component-icon-guide#): Programming and code (icon: binary-code)
- [Bug](https://neon.com/docs/community/component-icon-guide#): Debugging and testing (icon: bug)
- [CLI](https://neon.com/docs/community/component-icon-guide#): Command line interface (icon: cli)
- [CLI Cursor](https://neon.com/docs/community/component-icon-guide#): Terminal cursor (icon: cli-cursor)
- [Code](https://neon.com/docs/community/component-icon-guide#): Programming and development (icon: code)
- [GitHub](https://neon.com/docs/community/component-icon-guide#): Code repository (icon: github)
- [Hook](https://neon.com/docs/community/component-icon-guide#): Development hooks (icon: hook)
- [Ladder](https://neon.com/docs/community/component-icon-guide#): Development progression (icon: ladder)
- [Laptop](https://neon.com/docs/community/component-icon-guide#): Development machine (icon: laptop)
- [Setup](https://neon.com/docs/community/component-icon-guide#): Configuration and setup (icon: setup)
- [Wrench](https://neon.com/docs/community/component-icon-guide#): Tools and utilities (icon: wrench)

#### Business & Analytics

- [A Chart](https://neon.com/docs/community/component-icon-guide#): Analytics and charts (icon: a-chart)
- [App Store](https://neon.com/docs/community/component-icon-guide#): Application marketplace (icon: app-store)
- [Calendar Day](https://neon.com/docs/community/component-icon-guide#): Scheduling and time (icon: calendar-day)
- [Cards](https://neon.com/docs/community/component-icon-guide#): Card-based interface (icon: cards)
- [Check](https://neon.com/docs/community/component-icon-guide#): Verification and approval (icon: check)
- [Cheque](https://neon.com/docs/community/component-icon-guide#): Payment processing (icon: cheque)
- [Chart Bar](https://neon.com/docs/community/component-icon-guide#): Data visualization (icon: chart-bar)
- [CSV](https://neon.com/docs/community/component-icon-guide#): Data format (icon: csv)
- [Data](https://neon.com/docs/community/component-icon-guide#): Data and information (icon: data)
- [Metrics](https://neon.com/docs/community/component-icon-guide#): Performance metrics (icon: metrics)
- [Performance](https://neon.com/docs/community/component-icon-guide#): System performance (icon: performance)
- [Queries](https://neon.com/docs/community/component-icon-guide#): Database queries (icon: queries)
- [Research](https://neon.com/docs/community/component-icon-guide#): Development research (icon: research)
- [Stopwatch](https://neon.com/docs/community/component-icon-guide#): Timing and performance (icon: stopwatch)
- [Table](https://neon.com/docs/community/component-icon-guide#): Data tables (icon: table)
- [Todo](https://neon.com/docs/community/component-icon-guide#): Task management (icon: todo)
- [Transactions](https://neon.com/docs/community/component-icon-guide#): Database transactions (icon: transactions)
- [Trend Up](https://neon.com/docs/community/component-icon-guide#): Growth and trends (icon: trend-up)

#### UI & Design

- [Audio Jack](https://neon.com/docs/community/component-icon-guide#): Audio connectivity (icon: audio-jack)
- [Cards](https://neon.com/docs/community/component-icon-guide#): Card-based interface (icon: cards)
- [Gamepad](https://neon.com/docs/community/component-icon-guide#): Gaming and interaction (icon: gamepad)
- [GUI](https://neon.com/docs/community/component-icon-guide#): Graphical user interface (icon: gui)
- [Screen](https://neon.com/docs/community/component-icon-guide#): Display and interface (icon: screen)
- [Sparkle](https://neon.com/docs/community/component-icon-guide#): Visual effects (icon: sparkle)
- [User](https://neon.com/docs/community/component-icon-guide#): User interface (icon: user)

#### Infrastructure & Services

- [Autoscaling](https://neon.com/docs/community/component-icon-guide#): Automatic scaling (icon: autoscaling)
- [AWS](https://neon.com/docs/community/component-icon-guide#): Cloud computing platform (icon: aws)
- [Branching](https://neon.com/docs/community/component-icon-guide#): Database branching (icon: branching)
- [Database](https://neon.com/docs/community/component-icon-guide#): Database systems (icon: database)
- [Discord](https://neon.com/docs/community/component-icon-guide#): Communication platform (icon: discord)
- [Download](https://neon.com/docs/community/component-icon-guide#): File download (icon: download)
- [Enable](https://neon.com/docs/community/component-icon-guide#): Feature enablement (icon: enable)
- [Filter](https://neon.com/docs/community/component-icon-guide#): Data filtering (icon: filter)
- [Find Replace](https://neon.com/docs/community/component-icon-guide#): Search and replace (icon: find-replace)
- [Gear](https://neon.com/docs/community/component-icon-guide#): Settings and configuration (icon: gear)
- [Globe](https://neon.com/docs/community/component-icon-guide#): Global connectivity (icon: globe)
- [Handshake](https://neon.com/docs/community/component-icon-guide#): Partnerships and agreements (icon: handshake)
- [Heroku](https://neon.com/docs/community/component-icon-guide#): Cloud platform (icon: heroku)
- [Hourglass](https://neon.com/docs/community/component-icon-guide#): Time and waiting (icon: hourglass)
- [Import](https://neon.com/docs/community/component-icon-guide#): Data import (icon: import)
- [Invert](https://neon.com/docs/community/component-icon-guide#): Data transformation (icon: invert)
- [Lock Landscape](https://neon.com/docs/community/component-icon-guide#): Security and access control (icon: lock-landscape)
- [Neon](https://neon.com/docs/community/component-icon-guide#): Serverless Postgres (icon: neon)
- [Network](https://neon.com/docs/community/component-icon-guide#): Network connectivity (icon: network)
- [Postgres](https://neon.com/docs/community/component-icon-guide#): Database system (icon: postgres)
- [Prisma](https://neon.com/docs/community/component-icon-guide#): ORM for Node.js (icon: prisma)
- [Privacy](https://neon.com/docs/community/component-icon-guide#): Data privacy (icon: privacy)
- [Puzzle](https://neon.com/docs/community/component-icon-guide#): Problem solving (icon: puzzle)
- [Refresh](https://neon.com/docs/community/component-icon-guide#): Data refresh (icon: refresh)
- [Respond Arrow](https://neon.com/docs/community/component-icon-guide#): Response and feedback (icon: respond-arrow)
- [Scale Up](https://neon.com/docs/community/component-icon-guide#): Scaling and growth (icon: scale-up)
- [Search](https://neon.com/docs/community/component-icon-guide#): Search functionality (icon: search)
- [Split Branch](https://neon.com/docs/community/component-icon-guide#): Code branching (icon: split-branch)
- [SQL](https://neon.com/docs/community/component-icon-guide#): Database queries (icon: sql)
- [Unlock](https://neon.com/docs/community/component-icon-guide#): Access control (icon: unlock)
- [Vercel](https://neon.com/docs/community/component-icon-guide#): Deployment platform (icon: vercel)
- [Wallet](https://neon.com/docs/community/component-icon-guide#): Payment and billing (icon: wallet)
- [Warning](https://neon.com/docs/community/component-icon-guide#): Alerts and warnings (icon: warning)
- [X](https://neon.com/docs/community/component-icon-guide#): Close or cancel (icon: x)

---

## Summary

This guide provides an overview of Neon's icon systems and usage guidelines. For complete icon listings and detailed examples, refer to the [Component Guide](https://neon.com/docs/community/component-guide).

### Key takeaways

- **TechCards** uses colorful technology logos from `/public/images/technology-logos/`
- **DetailIconCards** uses monochrome feature icons mapped in component code
- **DocsList** uses built-in themes for simple navigation
- Always test icons in the target component before using
- Choose the right component based on your use case, not just the icon availability

---

## Related docs (Community)

- [Community hub](https://neon.com/docs/community/community-intro)
- [Docs contribution guide](https://neon.com/docs/community/contribution-guide)
- [Using Mermaid diagrams](https://neon.com/docs/community/mermaid-diagrams)
- [Component guide](https://neon.com/docs/community/component-guide)
- [Component specialized guide](https://neon.com/docs/community/component-specialized)
- [Component architecture](https://neon.com/docs/community/component-architecture)
- [AI tools for documentation](https://neon.com/docs/community/ai-tools)
- [Using docs as Markdown (LLMs)](https://neon.com/docs/community/llms-markdown-guide)
