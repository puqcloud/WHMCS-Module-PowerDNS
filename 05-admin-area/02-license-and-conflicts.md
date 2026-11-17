# License Alerts & Zone Conflicts

### PowerDNS module **[WHMCS](https://puqcloud.com/link.php?id=77)**
##### [Order now](https://puqcloud.com/whmcs-module-powerdns.php) | [Download](https://download.puqcloud.com/WHMCS/servers/PUQ_WHMCS-PowerDNS/) | [Community](https://community.puqcloud.com/)

---

## 1. Automated License Verification & Admin Alerts

The module validates licensing for each configured product against `license.puqcloud.com`.
- Verification transactions occur online periodically and are securely cached in the local database (`puq_license`), ensuring that temporary connectivity interruptions to the license server never disrupt customer DNS operations.
- If a product is configured with an invalid or expired license key, an alert notification is prominently displayed on the **WHMCS Admin Dashboard**. The alert lists each affected product and provides a direct shortcut link to its configuration page.

> **Note:** When a product's license is invalid or missing, automated module lifecycle commands (Create, Suspend, Unsuspend, Terminate) are halted, and a license validation warning is returned to prevent provisioning errors.

---

## 2. DNS Zone Conflicts Handling

A DNS zone must be strictly unique across an authoritative PowerDNS server. If a client attempts to create a zone that already exists on the server or is assigned to another WHMCS service:

1. **Client Experience**: The client does not encounter an unhandled PHP or API exception. Instead, a clean, user-friendly notification informs them that the zone cannot be added.
2. **Database Logging**: The attempted zone is logged in `puqPowerDNS_zones` with status `not allowed`.
3. **Automated Staff Task**: The module automatically registers a high-priority To-Do item in **WHMCS Admin** → **Utilities** → **To-Do List** titled `DNS zone conflict`. The task includes:
   - Client ID and Service ID
   - Conflicting Zone Name
   - Reason for conflict (already exists on PowerDNS server or across another service)
4. Staff can review the conflict, coordinate with the domain owner or remove stale orphan zones, and resolve the task.

---

## 3. Module Logging & Diagnostics

All outbound API calls to PowerDNS and licensing endpoints are recorded in **WHMCS Admin** → **Utilities** → **Logs** → **Module Log** under the identifier `puqPowerDNS`:
- To keep logs clean and readable, routine offline database license checks are suppressed from the module log.
- Full HTTP request endpoints, request payloads, response bodies, and HTTP status codes are recorded for API interactions, providing complete diagnostic visibility.
