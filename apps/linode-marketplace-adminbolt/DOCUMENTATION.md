---
title: "Deploy Adminbolt"
description: "Adminbolt is a modern, flexible Linux server control panel. Follow this guide to deploy Adminbolt on Akamai using Quick Deploy Apps."
published: 2026-05-28
modified: 2026-05-28
keywords: ['adminbolt','hosting control panel','marketplace apps', 'almalinux', 'web hosting', 'cpanel alternative']
tags: ["almalinux","cloud manager","linode platform","web hosting","control panel","quick deploy apps","ssl","web applications"]
external_resources:
- '[Adminbolt documentation](https://docs.adminbolt.com/)'
- '[Adminbolt website](https://adminbolt.com/)'
authors: ["Akamai"]
contributors: ["Akamai"]
license: '[CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0)'
marketplace_app_id:
marketplace_app_name: "Adminbolt"
---

[Adminbolt](https://adminbolt.com/) is a modern, flexible Linux server control panel. It lets you manage websites, domains, databases, email accounts, and SSL certificates from a clean web interface with no manual server configuration. The Marketplace app installs the full Adminbolt stack (web panel served by `bolt-nginx`, PHP via `bolt-php`, MariaDB, PowerDNS, Postfix, Dovecot, Redis, Rspamd, and Fail2Ban) and is fully unattended.

## Deploying a Quick Deploy App

{{< content "marketplace-deploy-shortguide" >}}

**Distribution:** AlmaLinux 9 \
**Suggested Plan:** Shared CPU - 4 GB Linode (or higher). Smaller plans may work for evaluation but the full stack benefits from at least 4 GB RAM.

## Configuration Options

Adminbolt requires no user-defined fields at deployment time. The app generates a one-time admin SSO login link automatically after installation and stores it on the server. No static admin password is set.

{{< content "marketplace-required-limited-user-shortguide" >}}

## Getting Started After Deployment

After deployment is complete, the Adminbolt admin panel is available on port `8443` via HTTPS. The app uses a single sign-on (SSO) flow for the administrator account, so there is no fixed admin password.

### Obtain the SSO Login URL

1.  Log in to your Linode through SSH as the `root` user, using the password set when you deployed the Linode:

    ```command
    ssh root@<Linode's IP address>
    ```

1.  Read the SSO link generated during installation:

    ```command
    cat /root/adminbolt-access.txt
    ```

    The file contains the panel URL and the one-time SSO URL.

1.  If the saved link has expired or you prefer to generate a fresh one, run:

    ```command
    bolt-cli admin-sso-generate
    ```

    This prints a fresh login URL that logs you straight into the panel.

### Accessing the Adminbolt Panel

1.  Open the SSO URL from the previous step in your web browser. It opens the Adminbolt panel at:

    ```
    https://<Linode's IP address>:8443
    ```

1.  On the first visit, your browser may warn about a self-signed certificate. This is expected. Add the temporary security exception and continue. After you attach your own domain, you can issue a trusted SSL certificate directly from the Adminbolt panel.

1.  The SSO URL logs the administrator in directly. The session is bound to that one-time link, so subsequent visits should be made through a fresh `bolt-cli admin-sso-generate` URL or by signing in with credentials you configure for additional users from the panel.

### First Steps in the Panel

1.  In the Adminbolt panel, add your first domain under the **Websites** section. Adminbolt automatically configures NGINX and PHP for the new site.

1.  Create a database for your application from the **Databases** section.

1.  Add the email accounts you need from the **Email** section (Postfix and Dovecot are already configured and managed by Adminbolt).

### Manually Configure a Domain

Adminbolt is most useful when reached through your own domain.

1.  Log in to your domain registrar (e.g. Namecheap, GoDaddy, Cloudflare). Create an `A` record that points your domain (and optionally `www.your-domain.tld`) to the public IPv4 address of your Linode.

1.  In the Adminbolt panel, open the **Websites** section and edit the website you added in the previous section. Confirm that the domain matches the `A` record you created.

1.  Once DNS has propagated (usually a few minutes to a few hours), use the Adminbolt panel to issue a free SSL certificate for the domain. Adminbolt uses Let's Encrypt under the hood and configures NGINX automatically.

### Useful Commands

Generate a new SSO login URL at any time:

```command
bolt-cli admin-sso-generate
```

Check the status of the panel services:

```command
systemctl status bolt-nginx
systemctl status bolt-php
```

Inspect the provisioning log produced when the Linode first booted:

```command
cat /var/log/stackscript.log
```

## Going Further

For more information about managing your Adminbolt server and its features, see the resources below:

-   [Adminbolt documentation](https://docs.adminbolt.com/)
-   [Adminbolt website](https://adminbolt.com/)
-   [Adminbolt installer source](https://github.com/AdminBolt/Installer)

For support specific to this Marketplace app deployment, contact the publisher through the support URL listed on the Marketplace listing.
