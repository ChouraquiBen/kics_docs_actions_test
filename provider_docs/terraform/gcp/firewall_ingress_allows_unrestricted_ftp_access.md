---
title: "Google compute firewall ingress allows unrestricted FTP access"
meta:
  name: "terraform/firewall_ingress_allows_unrestricted_ftp_access"
  id: "d3f8e9c1-7a2b-4d5f-90e2-123456789abc"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Networking and Firewall"
---
## Metadata
**Name:** `terraform/firewall_ingress_allows_unrestricted_ftp_access`
**Id:** `d3f8e9c1-7a2b-4d5f-90e2-123456789abc`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Networking and Firewall
## Description
Allowing ingress from `0.0.0.0/0` on port 21 (FTP) in a firewall rule (`source_ranges = ["0.0.0.0/0"]`) exposes the FTP service to the entire internet, significantly increasing the risk of unauthorized access and brute-force attacks. FTP traffic is often unencrypted, which could enable attackers to intercept credentials or exfiltrate sensitive data if unrestricted access is permitted. Restricting ingress to trusted IP ranges (e.g., `source_ranges = ["192.168.1.0/24"]`) reduces the attack surface and helps maintain data security.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_firewall)

## Non-Compliant Code Examples
```gcp
resource "google_compute_firewall" "bad_example" {
  name    = "bad-firewall"
  network = "default"

  allow {
    protocol = "tcp"
    ports    = ["21"]
  }

  source_ranges = ["0.0.0.0/0"] # Unrestricted ingress for FTP
}

```

## Compliant Code Examples
```gcp
resource "google_compute_firewall" "good_example" {
  name    = "good-firewall"
  network = "default"

  allow {
    protocol = "tcp"
    ports    = ["21"]
  }

  source_ranges = ["192.168.1.0/24"] # Restricted ingress for FTP
}

```