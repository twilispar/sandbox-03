> This page location: Backend > Neon Auth > Guides > Production checklist
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Step-by-step guide for configuring essential settings before launching an application with Neon Auth, including trusted domains, custom email providers, OAuth credentials, email verification, and security measures.

# Auth production checklist

Required configuration before launching with Neon Auth

**Note: Beta**

The **Neon Auth with Better Auth** is in Beta. Share your feedback on [Discord](https://discord.gg/92vNTzKDGp) or via the [Neon Console](https://console.neon.tech/app/projects?modal=feedback).

Complete these steps before taking your application to production with Neon Auth.

## Auth production checklist

- [ ] [1. Configure trusted domains](https://neon.com/docs/auth/guides/configure-domains)
    Add your production domain(s) to enable OAuth and email verification redirects.
- [ ] [2. Set up custom email provider](https://neon.com/docs/auth/production-checklist#email-provider)
    Replace shared SMTP (`auth@mail.myneon.app`) with your own email service for reliable delivery and higher limits. A custom email provider is also required if you want to use verification links instead of verification codes.
- [ ] [3. Customize application name](https://neon.com/docs/auth/production-checklist#application-name)
    Set the name your users see in user-facing auth messages. Applies to Neon Auth (Better Auth) integrations. Defaults to the Neon project name.
- [ ] [4. Configure OAuth credentials (if using OAuth)](https://neon.com/docs/auth/guides/setup-oauth#production-setup)
    Set up your own Google and GitHub OAuth apps to replace shared development keys.
- [ ] [5. Enable email verification (recommended)](https://neon.com/docs/auth/guides/email-verification)
    **Email verification is not enabled by default.** Since anyone can sign up for your application, enabling email verification adds an important verification step to ensure users own their email address.
- [ ] [6. Disable localhost access](https://neon.com/docs/auth/production-checklist#localhost-access)
    Disable the "Allow Localhost" setting in your project's **Settings** → **Auth** page. This setting is enabled by default for development but should be disabled in production to improve security.

## Email provider (#email-provider)

Neon Auth uses a shared SMTP provider (`auth@mail.myneon.app`) by default for development and testing. For production, configure your own email provider for better deliverability and higher sending limits.

### Configure custom SMTP

In your project's **Settings** → **Auth** page, configure your email provider:

1. Select **Custom SMTP provider**
2. Enter your SMTP credentials:
   - **Host**: Your SMTP server hostname (for example, `smtp.gmail.com`)
   - **Port**: SMTP port (typically `465` for SSL or `587` for TLS)
   - **Username**: Your SMTP username
   - **Password**: Your SMTP password or app-specific password
   - **Sender email**: Email address to send from
   - **Sender name**: Display name for sent emails
3. Click **Save**

### Email provider requirements

- **Verification links**: Require a custom email provider
- **Verification codes**: Work with shared or custom email providers
- **Password reset**: Works with shared or custom email providers

**Note:** The shared email provider (`auth@mail.myneon.app`) is suitable for development and testing. For production applications, use a custom email provider for better deliverability and to avoid rate limits.

## Application name (#application-name)

Neon Auth uses the application name in user-facing auth messages, such as verification emails and password resets. By default, this is set to the Neon project name. This setting is available for Neon Auth (Better Auth) integrations only.

To set a custom application name:

1. Go to **Auth** in your Neon project
2. Select the **Configuration** tab
3. In the **Project Info** panel, edit the **Application Name** field

Each branch manages its own application name independently, so preview and development branches can use different names than production.

You can also update the application name via the API. See [Update auth configuration](https://neon.com/docs/auth/guides/manage-auth-api#update-auth-configuration).

## Localhost access (#localhost-access)

The "Allow Localhost" setting in your project's **Settings** → **Auth** page is enabled by default to allow authentication requests from localhost during development.

### Disable for production

For production environments, disable this setting to improve security:

1. Go to **Settings** → **Auth** in your Neon project
2. Find the **Allow Localhost** toggle
3. Disable the toggle

**Important:** Only enable "Allow Localhost" for local development. Disabling this setting in production prevents unauthorized authentication requests from localhost, improving your application's security posture.

---

## Related docs (Guides)

- [Email verification](https://neon.com/docs/auth/guides/email-verification)
- [Set up OAuth](https://neon.com/docs/auth/guides/setup-oauth)
- [Password reset](https://neon.com/docs/auth/guides/password-reset)
- [User management](https://neon.com/docs/auth/guides/user-management)
- [Configure domains](https://neon.com/docs/auth/guides/configure-domains)
- [Webhooks](https://neon.com/docs/auth/guides/webhooks)
- [Troubleshooting](https://neon.com/docs/auth/troubleshooting)
- [Manage Auth via the API](https://neon.com/docs/auth/guides/manage-auth-api)
