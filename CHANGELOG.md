# Changelog

### PowerDNS module **[WHMCS](https://puqcloud.com/link.php?id=77)**
##### [Order now](https://puqcloud.com/whmcs-module-powerdns.php) | [Download](https://download.puqcloud.com/WHMCS/servers/PUQ_WHMCS-PowerDNS/) | [Community](https://community.puqcloud.com/)

## v4.0.0 — 16-11-2026

Major architecture update for ionCube 15 compatibility, modern Guzzle HTTP client integration, PUQ UI Toolkit client area upgrade, automated product settings migration, and enhanced security standards.

### Architecture & Compatibility
- **38 DNS Record Types Supported** — Full support across API, UI, and backend serialization for 38 DNS record types: `A`, `AAAA`, `AFSDB`, `ALIAS`, `CAA`, `CERT`, `CDNSKEY`, `CDS`, `CNAME`, `DNSKEY`, `DNAME`, `DS`, `HINFO`, `HTTPS`, `KEY`, `LOC`, `LUA`, `MX`, `NAPTR`, `NS`, `NSEC`, `NSEC3`, `NSEC3PARAM`, `OPENPGPKEY`, `PTR`, `RP`, `RRSIG`, `SMIMEA`, `SOA`, `SPF`, `SRV`, `SSHFP`, `SVCB`, `TKEY`, `TLSA`, `TSIG`, `TXT`, `URI`.
- **Apex & Provider NS Protection** — Apex root NS records matching provider nameservers are permanently locked with a lock icon in the UI and enforced in backend validation (`Record type not allowed`). Subdomain NS delegations remain fully editable and deletable by the client.
- **PowerDNS RFC Normalization** — Built-in RFC compliance and auto-canonicalization: strict RFC 1876 coordinate formatting for `LOC`, generic hex encoding for `TKEY`, parameter quote cleanup for `HTTPS`/`SVCB`, and automatic apex anchoring for DNSSEC records (`DNSKEY`, `CDNSKEY`, `CDS`, `NSEC3PARAM`).
- **High-Performance Batched Provisioning** — Template records during zone creation are batched into a single atomic PowerDNS PATCH request, reducing provisioning time from over 35 seconds to under 3 seconds.
- **Unencoded Hooks Architecture (v4.0.0+ / ionCube 15)** — `hooks.php` is now completely unencoded with zero business logic, ensuring 100% compatibility with ionCube 15 and WHMCS dynamic hook loading. All hook logic moved to `lib/puqPowerDNSHooks.php`.
- **Guzzle HTTP Client** — Replaced procedural cURL calls in both the PowerDNS API client and license verification with WHMCS built-in `\GuzzleHttp\Client`, featuring robust error handling, JSON encoding, and timeout controls.
- **Clean Module Logging** — Eliminated repetitive local offline database license checks from the WHMCS Module Log, recording exclusively online license transactions to prevent log clutter.
- **PHP 7.4 – 8.4 Support** — Full compatibility with PHP 7.4 through PHP 8.4, explicit nullable parameter types, and strict null safety.
- **WHMCS 9 & Select2 Dynamic Settings** — Modernized product settings dropdown detection to support WHMCS 9 Select2 elements seamlessly.

### Client Area & UI Toolkit
- **PUQ UI Toolkit** — Added standard `templates/header.tpl` providing CSS design tokens, modern card layouts, toast alerts, loading spinners, and clipboard helpers.
- **CSRF Protection** — Form and state-changing AJAX calls in the client area now include CSRF token verification.
- **Modern Dashboard UI** — Clean left-aligned layout, prominent top action card (`btn btn-outline-primary`), responsive 2-column detail grid, and optimized zones list table.
- **Full Localization** — Dual Ukrainian language file support (`ukrainian.php` and `ukranian.php`), sanitized typographically correct apostrophes across Catalan, French, and Italian, and 100% localized modals and placeholders across 26 languages.

### Settings & Provisioning
- **Allowed Record Types Restriction & Quick Presets** — Product settings now feature an `Allowed record types` configuration with quick preset buttons (All, Web Basics, Web + Mail, Standard, Clear) and interactive clickable badges for 24+ DNS record types. Leaving it empty allows all record types by default. In the client area, record creation (Add Record dropdown) and record editing (table and modal) are dynamically filtered and backend-enforced.
- **Admin Zone Sync & Server Pull/Push** — Added administrative controls on the service page to pull existing zones from the PowerDNS server, push local records to the server, inspect and delete records, and run interactive zone synchronization with conflict resolution.
- **Conflict Zone Isolation** — Hardened zone deletion, reload, and service termination to ensure `not allowed` conflict entries never modify or remove zones belonging to other active services on the PowerDNS server.
- **Zero-Touch Settings Auto-Migration** — Automatically upgrades legacy product settings (`configoption3/4/5/7`) into the modern unified configuration slot (`configoption24`) in the database upon product viewing.
- **Automated Service Cleanup** — Local zone database records are automatically cleaned up upon service termination to maintain database integrity.
- **Protective Directory Indexes** — Added protective `index.php` across all module subdirectories.

---

## v3.0

Full rewrite onto the unified PUQ WHMCS module skeleton.

- **Architecture:** license verification moved into the main module file with a block-based offline cache; product configuration consolidated into a single JSON document in `configoption24`, rendered by an `AdminAreaFooterOutput`-injected form. Backward compatible — v2.x installs keep working by reading the legacy `configoption3/4/5/7` slots until the first save through the new form.
- **Schema migrations:** introduced a versioned, idempotent migration runner (`puqPowerDNSMigrations`) recorded in `puq_module_versions`. Adds `created_at`, `updated_at` and `serial` columns to `puqPowerDNS_zones`. A fresh install and a v2 upgrade converge on the same schema.
- **Client area:** redesigned with a modern card-based UI. Creating, editing and deleting zones and records — plus zone reload — now run over AJAX with proper ownership checks (no more session-message redirects).
- **Admin area:** the service product tab lists zones with AJAX reload/delete actions; an admin homepage alert flags products with an invalid or missing license.
- **Hardening:** full pass for PHP 7.4 / 8.1 / 8.2+ compatibility — null-safe access to all request/DB data, `try/catch` around every external call (PowerDNS API, WHMCS API, database, cURL), consistent error logging, and `htmlspecialchars` on all output.
- **Localization:** 25 languages kept in sync against the canonical English key set.
- **Compatibility:** shipped as separate ionCube builds for PHP 7.4 (`php74`), 8.1 (`php81`) and 8.2+ (`php82`). Requires ionCube Loader v13+.
