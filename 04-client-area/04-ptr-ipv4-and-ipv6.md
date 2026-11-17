# PTR IPv4 and IPv6

### PowerDNS module **[WHMCS](https://puqcloud.com/link.php?id=77)**
##### [Order now](https://puqcloud.com/whmcs-module-powerdns.php) | [Download](https://download.puqcloud.com/WHMCS/servers/PUQ_WHMCS-PowerDNS/) | [Community](https://community.puqcloud.com/)

Pointer (**PTR**) resource records provide Reverse DNS resolution — translating an IP address back into a canonical fully qualified domain name (FQDN). PTR records are critical for email deliverability (SPF/DKIM/rDNS checks) and network diagnostic logging.
---

## 1. Understanding Reverse DNS Zones

Reverse DNS lookups query specialized reverse zone domains determined by the network subnet:

- **IPv4 Reverse Zones (`in-addr.arpa`)**: Constructed by reversing the octets of the network IP block and appending `.in-addr.arpa`.  
  *Example*: For subnet `203.0.113.0/24`, the reverse zone is `113.0.203.in-addr.arpa`.
- **IPv6 Reverse Zones (`ip6.arpa`)**: Constructed by reversing each individual hexadecimal nibble (4 bits) separated by dots and appending `.ip6.arpa`.  
  *Example*: For `2001:db8::/32`, the reverse zone is `8.b.d.0.1.0.0.2.ip6.arpa`.

---

## 2. Setting Up a Reverse Zone

1. Ensure your upstream transit provider or IP registrar (RIPE, ARIN, APNIC) has delegated the corresponding reverse DNS subnet to your PowerDNS name servers.
2. In the WHMCS client area, click **Add DNS Zone**.
3. In **Zone Name**, enter the reverse zone domain (e.g., `113.0.203.in-addr.arpa`).
4. Click **Add DNS Zone**. The zone is created on the server as an authoritative master zone.

---

## 3. Adding PTR Records

1. Open the reverse zone by clicking **Records**.
2. Click **Add Record** and select **Type**: `PTR`.
3. Fill in the parameters:
   - **Name**: The host identifier within the reverse zone:
     - For IPv4 host `203.0.113.10` inside zone `113.0.203.in-addr.arpa`, enter `10`.
     - For IPv6 host `2001:db8::1` inside zone `8.b.d.0.1.0.0.2.ip6.arpa`, enter the reversed host nibble sequence.
   - **TTL**: Choose a cache lifetime (e.g., `3600`).
   - **Domain Name**: The fully-qualified target domain name (e.g., `mail.example.com`).
4. Click **Add Record**. The module automatically appends trailing dots as needed.

---

## 4. Verification

Test your reverse DNS propagation using `dig` or `host`:

```bash
# Test IPv4 PTR Record
dig -x 203.0.113.10 +short

# Test IPv6 PTR Record
dig -x 2001:db8::1 +short
```

The output should return your target FQDN.
