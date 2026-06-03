> This page location: Data protection > IP Allow
> Full Neon documentation index: https://neon.com/docs/llms.txt

> Summary: Covers the setup of Neon's IP Allow feature to restrict database access to specified IP addresses, enhancing security by preventing unauthorized connections and allowing configuration for individual IPs or ranges.

# IP Allow

Limit database access to trusted IP addresses

Neon's IP Allow feature, available with the Neon [Scale](https://neon.com/docs/introduction/plans) plan, ensures that only trusted IP addresses can connect to the project where your database resides, preventing unauthorized access and helping maintain overall data security. You can limit access to individual IP addresses, IP ranges, or IP addresses and ranges defined with [CIDR notation](https://neon.com/docs/reference/glossary#cidr-notation).

You can configure **IP Allow** in your Neon project's settings. To get started, see [Configure IP Allow](https://neon.com/docs/manage/projects#configure-ip-allow).

![IP Allow configuration](https://neon.com/docs/manage/ip_allow.png)

## IP Allow together with Protected Branches

You can apply IP restrictions more precisely by designating specific branches in your Neon project as protected and enabling the **Restrict IP access to protected branches only** option. This will apply your IP allowlist to protected branches only with no IP restrictions on other branches in your project. Typically, the protected branches feature is used with branches that contain production or sensitive data. For step-by-step instructions, refer to our [Protected Branches guide](https://neon.com/docs/guides/protected-branches).

**Tip:** If you are an AWS user, Neon also supports a **Private Networking** feature, which enables connections to your Neon databases via AWS PrivateLink, bypassing the open internet entirely. See [Private Networking](https://neon.com/docs/guides/neon-private-networking).

---

## Related docs (Data protection)

- [Private Networking](https://neon.com/docs/guides/neon-private-networking)
- [Protected branches](https://neon.com/docs/guides/protected-branches)
