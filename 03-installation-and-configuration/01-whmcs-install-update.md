# WHMCS Installation and Update

### PowerDNS module **[WHMCS](https://puqcloud.com/link.php?id=77)**
##### [Order now](https://puqcloud.com/whmcs-module-powerdns.php) | [Download](https://download.puqcloud.com/WHMCS/servers/PUQ_WHMCS-PowerDNS/) | [Community](https://community.puqcloud.com/)

## System requirements

| Requirement | Minimum |
|-------------|---------|
| **WHMCS** | 8.x+, 9.x+ |
| **PHP** | 7.4, 8.1, 8.2, 8.3, 8.4 |
| **ionCube Loader** | v15+ |
| **PowerDNS Authoritative** | 4.x+ with HTTP API enabled |

> **Note:** The module uses ionCube encoding. Make sure ionCube Loader v15 or newer is installed and active on your server.

---

## Installation

> **Note:** The module uses **ionCube 15**, providing universal support across PHP 7.4 through PHP 8.4.
>
> All versions can be found at this link:  
> [https://download.puqcloud.com/WHMCS/servers/PUQ_WHMCS-PowerDNS/](https://download.puqcloud.com/WHMCS/servers/PUQ_WHMCS-PowerDNS/)
>
> Older module versions for WHMCS 8 are available in the archive directory:  
> [https://download.puqcloud.com/WHMCS/servers/PUQ_WHMCS-PowerDNS/archive/](https://download.puqcloud.com/WHMCS/servers/PUQ_WHMCS-PowerDNS/archive/)

1. Download the latest release from the PUQ download repository:
   ```bash
   wget https://download.puqcloud.com/WHMCS/servers/PUQ_WHMCS-PowerDNS/PUQ_WHMCS-PowerDNS-latest.zip
   ```

2. Unpack the downloaded archive:
   ```bash
   unzip PUQ_WHMCS-PowerDNS-latest.zip
   ```

3. Upload the `puqPowerDNS` folder into the `modules/servers/` directory of your WHMCS installation:
   ```bash
   cp -r puqPowerDNS /path/to/whmcs/modules/servers/
   ```

4. Ensure proper ownership and permissions for your web server user (`www-data`, `nginx`, or `apache`):
   ```bash
   chown -R www-data:www-data /path/to/whmcs/modules/servers/puqPowerDNS/
   chmod -R 755 /path/to/whmcs/modules/servers/puqPowerDNS/
   ```

5. The module files are now in place. On first load in WHMCS, the automated migration engine will construct all required tables (`puqPowerDNS_zones`, `puq_license`, `puq_module_versions`). No manual SQL execution is needed.

---

## Update

The update procedure consists of replacing existing module files with the newly downloaded release:

1. Back up your WHMCS database and `modules/servers/puqPowerDNS/` directory.
2. Download and unzip the latest release package.
3. Overwrite the files in `modules/servers/puqPowerDNS/` with the new version files.
4. Access any page in the WHMCS Admin Area. The module migration runner will automatically apply pending schema upgrades and log the new version in `puq_module_versions`.
5. Upgrading from legacy v2.x/v3.x releases is completely safe: existing product configurations are automatically migrated into the unified settings structure.
