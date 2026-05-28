# Linode adminbolt Deployment One-Click APP

adminbolt is an open-source Linux server control panel that simplifies hosting administration (websites, email, DNS, databases, and more) through a modern web interface. The installer deploys the full adminbolt stack (bolt-nginx, bolt-php, MariaDB, PowerDNS, Postfix, Dovecot, Redis, Rspamd, Fail2Ban, and more) and is fully unattended.

## Software Included

| Software   | Version   | Description              |
| :---       | :----     | :---                     |
| adminbolt  | latest    | Server control panel     |
| MariaDB    | latest    | Database server          |
| bolt-nginx | latest    | Panel web server (:8443) |

**Supported Distributions:**

- AlmaLinux 9

Note: adminbolt's installer supports AlmaLinux 9 only. AlmaLinux 8, Rocky, and Ubuntu are not supported by the upstream installer and will fail at bootstrap.

## Logging in

adminbolt uses single sign-on. There is no static admin password. After provisioning, the installer prints a one-time login URL for the web panel (https://`<ip>`:8443). To generate a fresh URL at any time, SSH to the Linode as `root` and run `bolt-cli admin-sso-generate`.

## Running the playbook manually

When deployed through the Marketplace, the StackScript (`deployment_scripts/linode-marketplace-adminbolt/adminbolt-deploy.sh`) writes the required `mode` variable to `group_vars/linode/vars` before invoking ansible-playbook. If you invoke the playbook directly (for local testing or `--check` dry runs), pass `mode` via extra-vars:

```
ansible-playbook site.yml -e "mode=production"
```

## Use our API

Customers can choose to deploy the adminbolt app through the Linode Marketplace or directly using the API. Before using the commands below, you will need to create an [API token](https://www.linode.com/docs/products/tools/linode-api/get-started/#create-an-api-token) or configure [linode-cli](https://www.linode.com/products/cli/) on an environment.

Make sure that the following values are updated at the top of the code block before running the commands:
- TOKEN
- ROOT_PASS

SHELL:
```
export TOKEN="YOUR API TOKEN"
export ROOT_PASS="aComplexP@ssword"

curl -H "Content-Type: application/json" \
-H "Authorization: Bearer $TOKEN" \
-X POST -d '{
    "backups_enabled": true,
    "booted": true,
    "image": "linode/almalinux9",
    "label": "adminboltlabel",
    "private_ip": false,
    "region": "us-central",
    "root_pass": "$ROOT_PASS",
    "stackscript_data": {},
    "stackscript_id": 0,
    "type": "g6-dedicated-4"
}' https://api.linode.com/v4/linode/instances
```

CLI:
```
export TOKEN="YOUR API TOKEN"
export ROOT_PASS="aComplexP@ssword"

linode-cli linodes create \
  --backups_enabled true \
  --booted true \
  --image 'linode/almalinux9' \
  --label adminboltlabel \
  --private_ip false \
  --region us-southeast \
  --root_pass '$ROOT_PASS' \
  --stackscript_data '{}' \
  --stackscript_id 0 \
  --type g6-dedicated-4
```

Replace `stackscript_id: 0` with the assigned marketplace StackScript ID once published.

## Resources

- [adminbolt documentation](https://docs.adminbolt.com/)
- [adminbolt installer source](https://github.com/AdminBolt/Installer)
- [Create Linode via API](https://www.linode.com/docs/api/linode-instances/#linode-create)
- [Stackscript reference](https://www.linode.com/docs/guides/writing-scripts-for-use-with-linode-stackscripts-a-tutorial/#user-defined-fields-udfs)

## Support

For help with this Marketplace app or with Adminbolt itself:

- Website: <https://adminbolt.com/>
- Documentation: <https://docs.adminbolt.com/>
- Billing and account: <https://billing.adminbolt.com/>
