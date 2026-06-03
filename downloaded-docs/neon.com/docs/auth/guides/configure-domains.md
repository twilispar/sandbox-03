> This page location: Backend > Neon Auth > Guides > Configure domains
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers trusted domains for Neon Auth: exact origins, wildcard preview domains, and enabling secure OAuth and email verification redirects.

# Configure trusted domains

Add your application domains to enable secure authentication redirects

**Note: Beta**

The **Neon Auth with Better Auth** is in Beta. Share your feedback on [Discord](https://discord.gg/92vNTzKDGp) or via the [Neon Console](https://console.neon.tech/app/projects?modal=feedback).

Add your application domains to Neon Auth's allowlist to enable OAuth and email verification redirects in production.

## Why domains are required

Neon Auth only redirects to domains in your allowlist. This prevents phishing attacks and unauthorized redirects by ensuring users are only sent to your legitimate application URLs.

Without adding your production domain, OAuth sign-in and verification links will fail when users try to access your application.

## Add a domain

1. Go to **Console → Auth → Configuration → Domains**
2. Enter your domain with protocol: `https://myapp.com`
3. Click **Add domain**

Repeat for each domain where your app runs.

**Note:** Include the protocol (`https://`) and omit trailing slashes. For example: `https://myapp.com` not `https://myapp.com/`

## Localhost is pre-configured

Development domains are automatically allowed, so you don't need to add them:

- `http://localhost:3000`
- `http://localhost:5173`
- Any `localhost` port

## Production domains

Add all domains where users access your application:

- `https://myapp.com`
- `https://www.myapp.com` (if you support www subdomain)
- `https://app.myapp.com` (if using a subdomain)

## Wildcard domains for previews

For preview environments with dynamic hostnames (for example Vercel preview deployments), you can add a **wildcard trusted domain** such as `https://*.my-app-preview.vercel.app`. One entry can match every preview under that pattern instead of adding hosts one by one.

Use the same rules as fixed domains: include `https://` (or `http://` where appropriate) and omit trailing slashes after the pattern.

**Note:** Wildcard patterns apply to the hostname segment you replace with `*`. Production apex domains (for example `https://myapp.com`) are usually still added as exact entries unless your wildcard covers them.

## Common issues

**Redirect blocked after OAuth sign-in:**

- Verify the domain is in your allowlist
- Ensure you included `https://` (not `http://` for production)
- Check spelling matches exactly (including www vs non-www)

**Verification link doesn't redirect:**

- Verification links use the same domain allowlist
- Add the domain where users should land after clicking the verification link

## Next steps

- [Production checklist](https://neon.com/docs/auth/production-checklist) - Complete setup for launch

---

## Related docs (Guides)

- [Email verification](https://neon.com/docs/auth/guides/email-verification)
- [Set up OAuth](https://neon.com/docs/auth/guides/setup-oauth)
- [Password reset](https://neon.com/docs/auth/guides/password-reset)
- [User management](https://neon.com/docs/auth/guides/user-management)
- [Webhooks](https://neon.com/docs/auth/guides/webhooks)
- [Production checklist](https://neon.com/docs/auth/production-checklist)
- [Troubleshooting](https://neon.com/docs/auth/troubleshooting)
- [Manage Auth via the API](https://neon.com/docs/auth/guides/manage-auth-api)
