---
title: "OSLogin Is Disabled For VM Instance"
meta:
  name: "terraform/os_login_is_disabled_for_vm_instance"
  id: "d0b4d550-c001-46c3-bbdb-d5d75d33f05f"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/os_login_is_disabled_for_vm_instance`
**Id:** `d0b4d550-c001-46c3-bbdb-d5d75d33f05f`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Configurations
## Description
This check ensures that the `enable-oslogin` metadata attribute is set to `true` on Google Compute Engine VM instances. Disabling OS Login (`enable-oslogin = "FALSE"`) allows users to manage SSH keys directly in instance metadata, which can lead to inconsistent access controls and make it harder to track or revoke user access. By setting `enable-oslogin` to `true`, as shown below, you centralize SSH access management through IAM, improving auditability and reducing the risk of unauthorized access.

```
metadata = {
  enable-oslogin = true
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_instance)

## Non-Compliant Code Examples
```gcp
resource "google_compute_instance" "positive1" {
  name         = "test"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  tags = ["foo", "bar"]

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
    }
  }

  // Local SSD disk
  scratch_disk {
    interface = "SCSI"
  }

  network_interface {
    network = "default"

    access_config {
      // Ephemeral IP
    }
  }

  metadata = {
    #... some other metadata

    enable-oslogin = "FALSE"
  }

  metadata_startup_script = "echo hi > /test.txt"

  service_account {
    scopes = ["userinfo-email", "compute-ro", "storage-ro"]
  }
}

```

## Compliant Code Examples
```gcp
resource "google_compute_instance" "negative1" {
  name         = "test"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  tags = ["foo", "bar"]

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
    }
  }

  // Local SSD disk
  scratch_disk {
    interface = "SCSI"
  }

  network_interface {
    network = "default"

    access_config {
      // Ephemeral IP
    }
  }

  metadata = {
    #... some other metadata

    # or if not undefined
    enable-oslogin = true
  }

  metadata_startup_script = "echo hi > /test.txt"

  service_account {
    scopes = ["userinfo-email", "compute-ro", "storage-ro"]
  }
}

```