### Terraform Network Booting

### Pre Requisites

Ubuntu MaaS Setup : ``
Ubuntu MaaS TFTP Setup: [pfSense - TFTP - MAAS](pfSense%20-%20TFTP%20-%20MAAS.md)

With Ubuntu MaaS setup and tftp settings setup to allow you to network boot any machines. you can follow along with these Terraform setup and apply configs. 

#### Configuring Terraform with Proxmox

As of the last update to this documentation the most recent Telmate/Proxmox plugin version for terraform is `2.9.11` to use this new version you will need a provider with the following structure.

```yml
terraform {
  required_version = ">=1.1.0"
  required_providers {
    proxmox = {
      source  = "telmate/proxmox"
      version = "2.9.11"
    }
  }
}

provider "proxmox" {
  pm_api_url          = "https://:8006/api2/json"
  pm_api_token_id     = ""
  pm_api_token_secret = ""
  pm_tls_insecure     = true
}
```