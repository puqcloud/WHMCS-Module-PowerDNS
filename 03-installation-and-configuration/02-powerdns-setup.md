# PowerDNS setup (HTTP API)

### PowerDNS module **[WHMCS](https://puqcloud.com/link.php?id=77)**
##### [Order now](https://puqcloud.com/whmcs-module-powerdns.php) | [Download](https://download.puqcloud.com/WHMCS/servers/PUQ_WHMCS-PowerDNS/) | [Community](https://community.puqcloud.com/)

The module communicates with your PowerDNS authoritative server via its built-in **HTTP API**. Before WHMCS can provision and manage DNS zones, you must enable the HTTP API and define an API key on your PowerDNS server.

---

## 1. Enable the HTTP API in PowerDNS

Open your PowerDNS main configuration file (typically `/etc/powerdns/pdns.conf` or a dedicated snippet file under `/etc/powerdns/pdns.d/`):

```ini
# Enable the built-in webserver and HTTP API
webserver=yes
webserver-address=0.0.0.0
webserver-port=8081
webserver-allow-from=0.0.0.0/0
api=yes
api-key=YOUR_SECRET_API_KEY_HERE
```

### Configuration Parameters:
- **`api-key`**: A cryptographically random secret string. This value will be entered into WHMCS as the **Server Password**.
- **`webserver-port`**: Listening port for HTTP API requests (default is `8081`). If using HTTPS reverse proxy (such as Nginx with SSL), use port `443` and enable the **Secure (SSL)** checkbox in WHMCS.
- **`webserver-allow-from`**: Comma-separated list or CIDR masks of allowed source IP addresses. Ensure the outbound IP address of your WHMCS server is included here.

Restart the PowerDNS service to apply the configuration:

```bash
systemctl restart pdns
```

---

## 2. Verify the HTTP API

From your WHMCS host (or any permitted workstation), test connectivity and authorization using `curl`:

```bash
curl -s -H 'X-API-Key: YOUR_SECRET_API_KEY_HERE' \
  http://YOUR_PDNS_HOST:8081/api/v1/servers/localhost | jq .
```

A successful response will return a JSON object with server attributes, including the `version` string:

```json
{
  "type": "Server",
  "id": "localhost",
  "url": "/api/v1/servers/localhost",
  "daemon_type": "authoritative",
  "version": "4.8.4",
  "zones_url": "/api/v1/servers/localhost/zones{/zone}"
}
```

This is the exact API endpoint WHMCS verifies when you click the **Test Connection** button.

---

## 3. Master and Zone Behavior Notes

- **Master Zone Architecture**: Zones provisioned by the module are registered as **Master** (`type: "Master"`) with `SOA-EDIT-API = INCREASE`. This ensures that PowerDNS automatically increments the SOA serial number on every record mutation.
- **SOA & NS Origin**: Authoritative name servers and default SOA parameters written to each zone originate strictly from your WHMCS **Product Configuration** (Module Settings), rather than global values in `pdns.conf`.
