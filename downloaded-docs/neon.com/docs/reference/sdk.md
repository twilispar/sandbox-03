> This page location: Tools & Workflows > API, CLI & SDKs > SDKs > Overview
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup of Client and Management SDKs for Neon, enabling application developers to implement user authentication and database queries, as well as manage platform resources programmatically.

# Neon SDKs

Neon provides two categories of SDKs to support different use cases:

- **Client SDKs**: For application developers building apps with the [Data API](https://neon.com/docs/data-api/overview) and optionally [Neon Auth](https://neon.com/docs/auth/overview). These SDKs handle database queries and user authentication from your application.
- **Management SDKs**: For programmatically managing Neon platform resources like projects, branches, databases, endpoints, and roles. These are wrappers around the [Neon API](https://api-docs.neon.tech/reference/getting-started-with-neon-api).

## Client SDKs

Use these SDKs to build applications with the Data API and optional authentication via Neon Auth or another JWT provider.

- [Neon TypeScript SDK](https://neon.com/docs/reference/javascript-sdk): Build apps with the Data API using database queries and authentication methods

## Management SDKs

Use these SDKs to programmatically manage your Neon infrastructure (projects, branches, databases, endpoints, roles, and operations).

- [Neon API TypeScript SDK](https://neon.com/docs/reference/typescript-sdk): Programmatically manage Neon projects, branches, databases, and other platform resources
- [Python SDK (Neon API)](https://neon.com/docs/reference/python-sdk): Programmatically manage Neon projects, branches, databases, and other platform resources
- [@neondatabase/toolkit](https://neon.com/docs/reference/neondatabase-toolkit): An SDK for AI Agents (and humans) that includes both the Neon API TypeScript SDK and the Neon Serverless Driver

## Community SDKs

**Note:** Community SDKs are not maintained or officially supported by Neon. Some features may be out of date, so use these SDKs at your own discretion. If you have questions about these SDKs, please contact the project maintainers.

- [Go SDK (Neon API)](https://github.com/kislerdm/neon-sdk-go): A Go SDK for the Neon API
- [Node.js / Deno SDK (Neon API)](https://github.com/paambaati/neon-js-sdk): A Node.js and Deno SDK for the Neon API

---

## Related docs (SDKs)

- [Neon TypeScript SDK](https://neon.com/docs/reference/javascript-sdk)
- [Neon API TypeScript SDK](https://neon.com/docs/reference/typescript-sdk)
- [Python SDK (Neon API)](https://neon.com/docs/reference/python-sdk)
- [The @neondatabase/toolkit](https://neon.com/docs/reference/neondatabase-toolkit)
- [Go SDK (Neon API)](https://neon.com/docs/https://github.com/kislerdm/neon-sdk-go)
- [Node.js / Deno SDK (Neon API)](https://neon.com/docs/https://github.com/paambaati/neon-js-sdk)
