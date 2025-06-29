---
title: "VM With Full Cloud Access"
meta:
  name: "terraform/vm_with_full_cloud_access"
  id: "bc280331-27b9-4acb-a010-018e8098aa5d"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/vm_with_full_cloud_access`
**Id:** `bc280331-27b9-4acb-a010-018e8098aa5d`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
If a Google Compute Engine VM instance is configured to use the default service account with `cloud-platform` scope (full access to all Cloud APIs), any process running on that instance can interact with all enabled Google Cloud services in the project, significantly increasing the risk of privilege escalation or unintended data exposure. For example, the following configuration is insecure:

```
service_account {
  scopes = ["userinfo-email", "compute-ro", "storage-ro", "cloud-platform"]
}
```

To limit permissions and reduce the attack surface, the service account should only be granted the minimal set of scopes necessary, such as:

```
service_account {
  scopes = ["userinfo-email", "compute-ro", "storage-ro"]
}
```

Leaving excessive permissions unaddressed can allow attackers or compromised applications to gain broad and unnecessary access across your cloud environment.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_instance#scopes)

## Non-Compliant Code Examples
```gcp
resource "google_compute_instance" "positive1" {
  name         = "test"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
    }
  }

  network_interface {
    network = "default"
    access_config {
      // Ephemeral IP
    }
  }

  service_account {
    scopes = ["userinfo-email", "compute-ro", "storage-ro", "cloud-platform"]
  }
}
```

## Compliant Code Examples
```gcp
resource "google_compute_instance" "negative1" {
  name         = "test"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
    }
  }

  network_interface {
    network = "default"
    access_config {
      // Ephemeral IP
    }
  }

  service_account {
    scopes = ["userinfo-email", "compute-ro", "storage-ro"]
  }
}
```