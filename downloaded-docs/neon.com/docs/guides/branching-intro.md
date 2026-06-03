> This page location: Branching > Get started with branching
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup and integration of Neon's branching feature into development workflows, including automation with APIs, CLI, and CI/CD tools for efficient branch management and preview deployments.

# Get started with branching

Everything you need to get started with Neon's branching feature

Find detailed information and instructions about Neon's branching feature and how you can integrate branching with your development workflows.

## What is branching?

Learn about branching and how you can apply it in your development workflows.

- [Learn about branching](https://neon.com/docs/introduction/branching): Learn about Neon's branching feature and how to use it in your development workflows
- [Database branching for Postgres](https://neon.com/blog/database-branching-for-postgres-with-neon): Blog: Read about how Neon's branching feature works and what it means for your workflows
- [Branch archiving](https://neon.com/docs/guides/branch-archiving): Learn how Neon automatically archives inactive branches to cost-effective storage
- [Schema-only branches](https://neon.com/docs/guides/branching-schema-only): Learn how you can protect sensitive data with schema-only branches
- [Branching authentication](https://neon.com/docs/auth/branching-authentication): Test sign-in, OAuth, and permissions in isolated branches without touching production

## Automate branching

Integrate branching into your CI/CD pipelines and workflows with the Neon API, CLI, GitHub Actions, and Githooks.

- [Branching with the Neon API](https://neon.com/docs/guides/branching-neon-api): Learn how to instantly create and manage branches with the Neon API
- [Branching with the Neon CLI](https://neon.com/docs/guides/branching-neon-cli): Learn how to instantly create and manage branches with the Neon CLI
- [Branching with GitHub Actions](https://neon.com/docs/guides/branching-github-actions): Automate branching with Neon's GitHub Actions for branching
- [Branching with Githooks](https://neon.com/blog/automating-neon-branch-creation-with-githooks): Blog: Learn how to automating branch creation with Githooks

## Preview deployments

Create a branch for each preview deployment with the [Neon-managed Vercel integration](https://neon.com/docs/guides/neon-managed-vercel-integration).

- [The Neon-Managed Vercel Integration](https://neon.com/docs/guides/neon-managed-vercel-integration): Connect your Vercel project and create a branch for each preview deployment
- [Preview deployments with Vercel](https://neon.com/blog/neon-vercel-integration): Blog: Read about full-stack preview deployments using the Neon Vercel Integration
- [A database for every preview](https://neon.com/blog/branching-with-preview-environments): Blog: A database for every preview environment with GitHub Actions and Vercel

## Test queries

Test potentially destructive or performance-impacting queries before your run them in production.

- [Branching — Testing queries](https://neon.com/docs/guides/branching-test-queries): Instantly create a branch to test queries before running them in production

## Data recovery and audits

Recover lost data or track down issues by restoring a branch to its history, or just create a point-in-time branch for historical analysis or any other reason.

- [Instant restore with Time Travel Assist](https://neon.com/docs/guides/branch-restore): Learn how to instantly recover your database to any point in time within your [history window](https://neon.com/docs/introduction/history-window)
- [Time Travel](https://neon.com/docs/guides/time-travel-assist): Query point-in-time connections with Time Travel 
- [Schema diff](https://neon.com/docs/guides/schema-diff): Visualize schema differences between branches to help with troubleshooting

## Example applications

Explore example applications that use Neon's branching feature.

- [Time Travel Demo](https://github.com/kelvich/branching_demo_bisect): Use Neon branching, the Neon API, and a bisect script to recover lost data
- [Preview branches app](https://github.com/neondatabase/preview-branches-with-vercel): An application demonstrating using GitHub Actions with preview deployments in Vercel

---

## Related docs (Branching)

- [About branching](https://neon.com/docs/introduction/branching)
- [Branching workflow primer](https://neon.com/docs/get-started/workflow-primer)
- [Branching workflows](https://neon.com/docs/guides/branching-test-queries)
- [Branch archiving](https://neon.com/docs/guides/branch-archiving)
- [Branch expiration](https://neon.com/docs/guides/branch-expiration)
- [Schema-only branches](https://neon.com/docs/guides/branching-schema-only)
- [Reset from parent](https://neon.com/docs/guides/reset-from-parent)
