> This page location: Plans and billing > Manage billing
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the management of billing in Neon, including accessing the Billing page, updating payment methods, downloading invoices, changing plans, and account deletion.

# Manage billing

Invoices, payment methods, changing your plan, and other actions around managing your bill

**What you will learn:**

- How to access the Billing page
- How to update payment method, billing email, and billing details
- How to download invoices
- How to request billing support
- How to change plans
- How to prevent further monthly charges
- How to delete your account

**Related topics**

- [Neon plans](https://neon.com/docs/introduction/plans)
- [Monitoring billing and usage](https://neon.com/docs/introduction/monitor-usage)
- [Support](https://neon.com/docs/introduction/support)

## View the Billing page

You can view and manage billing from the **Billing** page in the Neon Console.

To access your **Billing** page:

1. Navigate to the Neon Console.
2. Select your organization from the breadcrumb menu at the top-left of the console.
3. Select **Billing** from the menu to view the charges to date.

The **Billing** page has a sidebar with **Billing summary** and **Payment info**. **Billing summary** shows **This month's summary**: your billing period, plan details (including **Change plan**), and **Charges to date**. **Payment info** shows how you pay, your billing email, and company or address details (see [Update your payment method](https://neon.com/docs/introduction/manage-billing#update-your-payment-method) and [Update billing details](https://neon.com/docs/introduction/manage-billing#update-billing-details-company-and-address)).

At the top of the page, **View past invoices** opens your invoices. If you are on a **paid** plan, **Request billing support** appears next to it (see [Request billing support](https://neon.com/docs/introduction/manage-billing#request-billing-support)).

## Update your payment method

The **Payment info** view explains that **charges are applied to your card on the first day of the month**.

To update your payment method:

1. Navigate to the Neon Console.
2. Select your organization from the breadcrumb menu at the top-left of the console.
3. Select **Billing** from the menu.
4. In the sidebar, select **Payment info**.
5. In the **Payment method** card, select **Edit**.

If you are unable to update your payment method, please [contact support](https://neon.com/docs/introduction/support).

## Payment issues

### Missed payments

If an auto-debit payment transaction fails, Neon sends a request to update your payment method. Late fees and payment policies are described in the [Neon Platform Terms](https://neon.com/platform-terms).

### Failing payments for Indian customers

Neon's billing system uses **Stripe Checkout**, which does not currently support **e-Mandates** (a requirement from the Reserve Bank of India) (RBI) for automatic recurring payments. Because of this, customers in India cannot set up automatic monthly payments. In the event of a payment failure, please [contact support](https://neon.com/docs/introduction/support) to request a link to your invoice to complete the payment manually.

## Update your billing email

To update your billing email:

1. Navigate to the Neon Console.
2. Select your organization from the breadcrumb menu at the top-left of the console.
3. Select **Billing** from the menu.
4. In the sidebar, select **Payment info**.
5. In the **Billing email** card, select **Edit**.

If you are unable to update your billing email, please [contact support](https://neon.com/docs/introduction/support).

## Update billing details (company and address)

To update the company name, address, postal or ZIP code, country, or VAT or tax ID shown on your account:

1. Navigate to the Neon Console.
2. Select your organization from the breadcrumb menu at the top-left of the console.
3. Select **Billing** from the menu.
4. In the sidebar, select **Payment info**.
5. In the **Billing Info** card, select **Edit**.

If you are unable to update this information, please [contact support](https://neon.com/docs/introduction/support).

## Invoices

A Neon invoice includes the charges and the amount due for the billing period. For an explanation of what you've been billed for, see [Usage metrics](https://neon.com/docs/introduction/plans#usage-metrics).

### Download invoices

To download an invoice:

1. Navigate to the Neon Console.
2. Select your organization from the breadcrumb menu at the top-left of the console.
3. Select **Billing** from the menu.
4. Select **View past invoices**.
5. Find the invoice you want and open its actions menu, then select **Download**.

**Note:** When an invoice is paid, Neon's billing system sends a payment confirmation email to the address associated with the Neon account.

### Request billing support

If you have a question or problem with billing or an invoice, you can contact the billing team from the **Billing** page. This is available on **paid** plans.

#### From the Billing page header

1. Navigate to the Neon Console.
2. Select your organization from the breadcrumb menu at the top-left of the console.
3. Select **Billing** from the menu.
4. Select **Request billing support**.
5. In the form, choose the **related invoice** (if you have invoices) and describe how we can help, then submit.

If you have no invoices yet, **Request billing support** may appear disabled with a short explanation.

#### From past invoices

1. On the **Billing** page, select **View past invoices**.
2. For the invoice you care about, open the actions menu (`...`) and select **Request support**. The same form opens with that invoice pre-selected.

You can still use **Download** from the invoice menu to save a PDF. For other support paths (for example **Launch** plan limits on ticket topics), see [Support](https://neon.com/docs/introduction/support).

## Change your plan

**Note: Restart required for new limits**

If you're upgrading your plan, your compute will only pick up the new plan limits (such as max compute size and project storage) after the compute restarts. See [Restart a compute](https://neon.com/docs/manage/computes#restart-a-compute).

To upgrade or downgrade your plan:

1. Navigate to the Neon Console.
2. Select your organization from the breadcrumb menu at the top-left of the console.
3. Select **Billing** from the menu.
4. Select **Change plan**.

Changing your plan to one with lower usage allowances may affect the performance of your applications. To compare plan allowances, see [Neon plans](https://neon.com/docs/introduction/plans#neon-plans).

If you are downgrading your plan, you will be required to remove any projects, branches, or data that exceed your new plan allowances.

To downgrade from a **legacy Enterprise plan**, please open a support ticket. Cancellation of a legacy Enterprise plan is handled according to the Master Subscription Agreement (MSA) outlined in the Customer Agreement.

## How to prevent further monthly charges to your account

If you're on a Neon paid plan, you need to downgrade to the Free plan to avoid further monthly charges. You can do so from the [Billing](https://console.neon.tech/app/billing#change_plan) page in the Neon Console. Simply removing all Neon projects will **not** stop the monthly fee associated with your plan. You will continue to be invoiced until you downgrade to Free.

## Delete your account

If you would like to delete your Neon account entirely, please refer to the steps described here: [Deleting your account](https://neon.com/docs/manage/accounts#delete-account).

---

## Related docs (Plans and billing)

- [Plans and billing](https://neon.com/docs/introduction/about-billing)
- [Plans](https://neon.com/docs/introduction/plans)
- [Agent plan](https://neon.com/docs/introduction/agent-plan)
- [Spending limit](https://neon.com/docs/introduction/spending-limit)
- [Monitor billing](https://neon.com/docs/introduction/monitor-usage)
- [Usage and cost calculations](https://neon.com/docs/introduction/usage-calculations)
- [Cost optimization](https://neon.com/docs/introduction/cost-optimization)
- [Network transfer costs](https://neon.com/docs/introduction/network-transfer)
- [AWS Marketplace](https://neon.com/docs/introduction/billing-aws-marketplace)
