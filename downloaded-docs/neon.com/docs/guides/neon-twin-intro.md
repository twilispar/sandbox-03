> This page location: Tools & Workflows > Workflows & CI/CD > Neon Twin
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the creation of a Neon Twin, a clone of your production database, enabling developers to work in isolated environments with automatic synchronization and efficient management for development and testing workflows.

# Create a Neon Twin

Learn how to Twin your production database with Neon

> **Explore our dev/test use case**
>
> Move development and testing to Neon; keep production right where it is.Read more about our dev/test use case [here](https://neon.com/use-cases/dev-test).

## What is a Neon Twin?

A Neon Twin is a full or partial clone of your production or staging database, providing developers and teams with isolated, sandboxed environments that closely mirror production.

![Dev/Test Twin Workflow](https://neon.com/use-cases/dev-test-twin-workflow.png)

## Designed for efficiency

Creating a Neon Twin will streamline development workflows, enhance productivity, and help teams ship faster, all while being more cost-effective and easier to manage than traditional development/testing environments.

## Automatically synced

The workflows in this section enable automatic synchronization between your production database and your Neon Twin.

## Instant Branches

With a Neon Twin created, [branches](https://neon.com/docs/introduction/branching) can be quickly spun up or torn down, enabling developers to build new features or debug issues, all within their own isolated environments with a dedicated compute resource.

Branches can be created and managed through the [Neon console](https://console.neon.tech/) or programmatically via the [API](https://neon.com/docs/reference/api-reference).

## Get started

Pick the workflow that fits your needs. A full Twin mirrors your entire production database; a partial Twin clones only the schema and selected tables, which is faster and useful when you don't need every row of production data.

- [Create a full Twin](https://neon.com/docs/guides/neon-twin-full-pg-dump-restore): Clone your entire production database to Neon using pg_dump and pg_restore in a GitHub Actions workflow.
- [Create a partial Twin](https://neon.com/docs/guides/neon-twin-partial-pg-dump-restore): Clone only the schema and selected tables from production using pg_dump, pg_restore, and psql.

---

## Related docs (Workflows & CI/CD)

- [Branching with the CLI](https://neon.com/docs/guides/branching-neon-cli)
- [Branching with the API](https://neon.com/docs/guides/branching-neon-api)
- [Branching with GitHub Actions](https://neon.com/docs/guides/branching-github-actions)
- [Branching with CircleCI](https://neon.com/docs/guides/branching-circleci)
- [Claimable Postgres](https://neon.com/docs/reference/claimable-postgres)
- [Terraform](https://neon.com/docs/reference/terraform)
- [Data anonymization](https://neon.com/docs/workflows/data-anonymization)
