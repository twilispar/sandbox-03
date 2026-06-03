> This page location: Plans and billing > Monitor billing
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Learn where to monitor usage and costs for your Neon account: the Billing page (account-level), Projects page (org-level), Project dashboard (project-level), and the consumption metrics API for usage-based plans.

# Monitor billing and usage

Where to see usage and costs in the Console and via the API

You can monitor usage and costs in the Neon Console or programmatically with the Neon API. For what each metric means and how it maps to your invoice, see [Usage metrics](https://neon.com/docs/introduction/plans#usage-metrics) and [Invoice metrics](https://neon.com/docs/introduction/plans#invoice-metrics) on the Plans page.

## View usage in the Neon Console

Neon exposes usage in three places in the Console: the **Billing** page (account-level charges and plan summary), the **Projects** page (org-level usage metrics), and the **Project dashboard** (project-level usage metrics when you are inside a project).

### Billing page

From the **Billing** page (Organization → **Billing** in the Neon Console) you see account-level information: this month's summary including current plan, included features, usage-based pricing, and **charges to date**. Invoice line items appear only when there is a charge for that metric.

To open the Billing page:

1. Navigate to the Neon Console.
2. Select your organization from the breadcrumb menu at the top-left.
3. Select **Billing**.

### Projects page

From the **Projects** page (Organization → **Projects**) you see an org-level summary of four metrics across all projects: **Compute**, **Storage**, **History**, and **Network transfer**.

### Project dashboard

When you open a project, the **Project dashboard** (Project → **Dashboard**) shows the same four metrics for that project only: **Compute**, **Storage**, **History**, and **Network transfer**.

Usage is shown since the start of the current billing period. Metrics may be delayed by about an hour and are not updated for inactive projects.

**Note:** Network transfer metrics only appear on the Billing page when your usage exceeds the included allowance for your plan. To track network transfer before it results in charges, check the account usage panel on the **Projects** page, which always displays current usage. You can also monitor programmatically with the [Consumption API](https://neon.com/docs/guides/consumption-metrics). For more on network transfer, see [Network transfer](https://neon.com/docs/introduction/network-transfer).

**Note: Billing metrics for pre-2025 custom contract customers**

If you signed a contract with Neon prior to 01/01/2025, different billing metrics apply:

- **Storage** is measured in GiBs instead of [GB-month](https://neon.com/docs/reference/glossary#gb-month), and if you exceed your contract's monthly storage allowance, extra storage units are automatically allocated and billed. Extra storage charges are applied based on the number of additional storage units needed to cover peak storage usage during the current billing period, prorated from the date the extra storage was allocated. Peak usage resets at the beginning of the next billing period.
- **Written data** is the total volume of data written from compute to storage during the monthly billing period, measured in gibibytes (GiB).

If you have questions or want to change the billing metrics defined in your contract, contact your Neon sales representative.

## Retrieve usage with the API

On **usage-based plans** (Launch, Scale, Agent, and Enterprise), use the **project consumption metrics** API. It returns metrics that align with usage-based billing and match your invoice.

See [Querying consumption metrics](https://neon.com/docs/guides/consumption-metrics) for the endpoint, required and optional parameters, example requests and responses, pagination, polling behavior, and building usage dashboards. For a guide to converting the raw API values into billing units and calculating your costs, see [Usage and cost calculations](https://neon.com/docs/introduction/usage-calculations).

**Tip: Optimize your costs**

For strategies to reduce your Neon costs across compute, storage, branches, and data transfer, see [Cost optimization](https://neon.com/docs/introduction/cost-optimization).

---

## Related docs (Plans and billing)

- [Plans and billing](https://neon.com/docs/introduction/about-billing)
- [Plans](https://neon.com/docs/introduction/plans)
- [Agent plan](https://neon.com/docs/introduction/agent-plan)
- [Manage billing](https://neon.com/docs/introduction/manage-billing)
- [Spending limit](https://neon.com/docs/introduction/spending-limit)
- [Usage and cost calculations](https://neon.com/docs/introduction/usage-calculations)
- [Cost optimization](https://neon.com/docs/introduction/cost-optimization)
- [Network transfer costs](https://neon.com/docs/introduction/network-transfer)
- [AWS Marketplace](https://neon.com/docs/introduction/billing-aws-marketplace)
