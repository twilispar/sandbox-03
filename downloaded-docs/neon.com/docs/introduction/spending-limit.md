> This page location: Plans and billing > Spending limit
> Full Neon documentation index: https://neon.com/docs/llms.txt

# Spending limit

Manage your organization's spending with better visibility and control

**What you will learn:**

- What a spending limit does
- Who can set one
- How to enable, edit, and disable a limit in the Console
- How to manage limits with the Neon Management API
- What happens when the limit is reached

**Related topics**

- [Manage billing](https://neon.com/docs/introduction/manage-billing)
- [Monitor billing and usage](https://neon.com/docs/introduction/monitor-usage)
- [Cost optimization](https://neon.com/docs/introduction/cost-optimization)
- [Neon API keys](https://neon.com/docs/manage/api-keys#create-an-api-key)

## About spending limits

A spending limit helps you control your organization's Neon spending. When charges approach the limit, organization admins receive email alerts so they can take action before the bill grows further.

Spending limits are available on the Launch and Scale plans. You can manage them from the **Billing** page in the Neon Console or with the [Neon Management API](https://neon.com/docs/introduction/spending-limit#manage-spending-limits-with-the-neon-api).

**Coming Soon: Automatic project suspension**

Currently, email alerts are the only available action. Automatic project suspension is coming soon: When the limit is reached, projects' computes will pause until you raise the limit or the next monthly billing period begins.

## Who can set a spending limit

Only organization admins can enable, edit, or disable a spending limit. Other members see the limit in read-only form on the **Billing** page.

## Manage spending limits with the Neon API

The Management API exposes spending limits at:

`https://console.neon.tech/api/v2/organizations/{org_id}/billing/spending_limit`

Replace `{org_id}` with your organization ID (see [Finding your org_id](https://neon.com/docs/manage/orgs-api#finding-your-org_id)). Authenticate with a [personal API key](https://neon.com/docs/manage/api-keys#create-an-api-key) or another allowed credential for the Management API.

| Action                  | Method                                                                                                                  | Who can use it                                               |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Read the current limit  | [Retrieve the organization's monthly spending limit](https://api-docs.neon.tech/reference/getorganizationspendinglimit) | Organization members with **read** access (Launch and Scale) |
| Set or change the limit | [Set the organization's monthly spending limit](https://api-docs.neon.tech/reference/setorganizationspendinglimit)      | **Organization admins** only (Launch and Scale)              |
| Remove the limit        | [Clear the organization's monthly spending limit](https://api-docs.neon.tech/reference/deleteorganizationspendinglimit) | **Organization admins** only (Launch and Scale)              |

**Request body (`PUT`):** send `spending_limit_cents` as a positive integer (monthly cap in **cents**; minimum **1**). For example, `$100.00` per month is `10000`. Values **`0`** and **`null`** are rejected; to clear a limit, call **`DELETE`** on the same path (idempotent when no limit is configured).

**Response (`GET` / `PUT` 200):** the API returns `spending_limit_cents`, or `null` when no limit is set. Alerts at 80% and 100% of the cap behave the same as for Console-managed limits; computes are not suspended by this feature.

See the linked API reference pages for response codes, schemas, and **Try it** examples.

## Alert thresholds

When a spending limit is set, Neon sends email alerts to organization admins at two thresholds:

- When spending reaches 80% of the limit
- When spending reaches 100% of the limit

Neon checks your spending every 15 minutes. Once spending crosses a threshold, an alert email goes out, and reminders continue weekly until you raise the limit or the billing cycle resets.

## Enable a spending limit

1. Navigate to the [Neon Console](https://console.neon.tech).
2. Select your organization from the breadcrumb menu at the top-left.
3. Select **Billing** from the menu.
4. On the **Spending limit** card, select **Enable**.
5. In the **Enable spending limit** dialog, enter a whole-dollar amount.
6. Select **Enable**.

The **Spending limit** card now shows the current spending and a progress bar toward the limit.

## Edit the limit

1. Navigate to the **Billing** page.
2. On the **Spending limit** card, select **Edit**.
3. Update the amount and select **Save**.

You can also open the edit dialog from the over-limit banner by selecting **Manage spending limit**. The banner appears in the Console when spending is approaching the limit or has reached it.

## Disable the limit

1. Navigate to the **Billing** page.
2. On the **Spending limit** card, select **Disable**.
3. Confirm in the **Disable spending limit** dialog.

After you disable the limit, no further email alerts are sent and the over-limit banner no longer appears.

## What happens when the limit is reached

When your organization's monthly charges reach the limit:

- Admins receive a final email alert.
- A banner appears in the Neon Console showing **Limit reached**, with a **Manage spending limit** link that opens the edit dialog.

Projects continue to run, and charges continue to accumulate, until you raise the limit or the billing cycle resets at the start of the next month. Automatic project suspension is coming soon and will stop compute charges at the limit.

**Note:** A spending limit applies to the organization's total monthly Neon charges across all projects in that organization. If you belong to multiple organizations, set a separate limit for each.

---

## Related docs (Plans and billing)

- [Plans and billing](https://neon.com/docs/introduction/about-billing)
- [Plans](https://neon.com/docs/introduction/plans)
- [Agent plan](https://neon.com/docs/introduction/agent-plan)
- [Manage billing](https://neon.com/docs/introduction/manage-billing)
- [Monitor billing](https://neon.com/docs/introduction/monitor-usage)
- [Usage and cost calculations](https://neon.com/docs/introduction/usage-calculations)
- [Cost optimization](https://neon.com/docs/introduction/cost-optimization)
- [Network transfer costs](https://neon.com/docs/introduction/network-transfer)
- [AWS Marketplace](https://neon.com/docs/introduction/billing-aws-marketplace)
