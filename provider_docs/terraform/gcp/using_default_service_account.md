---
title: "Using Default Service Account"
meta:
  name: "terraform/using_default_service_account"
  id: "3cb4af0b-056d-4fb1-8b95-fdc4593625ff"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Defaults"
---
## Metadata
**Name:** `terraform/using_default_service_account`
**Id:** `3cb4af0b-056d-4fb1-8b95-fdc4593625ff`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Defaults
## Description
Google Compute Engine instances should not be configured to use the default service account, which has broad permissions and full access to all Cloud APIs. If the attribute `service_account`—specifically the `email` sub-attribute—is missing, empty, or set to the default Google Compute Engine service account, it increases the risk of privilege escalation and unauthorized access to sensitive resources. Instead, instances should explicitly specify a custom service account with only the necessary permissions, like in the following example:

```
service_account {
  email  = "custom-sa@project-id.iam.gserviceaccount.com"
  scopes = ["userinfo-email", "compute-ro", "storage-ro"]
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_instance)

## Non-Compliant Code Examples
```gcp
#this is a problematic code where the query should report a result(s)
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

  network_interface {
    network = "default"

    access_config {
      // Ephemeral IP
    }
  }

}

resource "google_compute_instance" "positive2" {
  name         = "test"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  tags = ["foo", "bar"]

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

resource "google_compute_instance" "positive3" {
  name         = "test"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  tags = ["foo", "bar"]

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
    email = ""
    scopes = ["userinfo-email", "compute-ro", "storage-ro"]
  }
}

resource "google_compute_instance" "positive4" {
  name         = "test"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  tags = ["foo", "bar"]

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
    email = "a"
    scopes = ["userinfo-email", "compute-ro", "storage-ro"]
  }
}

resource "google_compute_instance" "positive5" {
  name         = "test"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  tags = ["foo", "bar"]

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
    email = "email@developer.gserviceaccount.com"
    scopes = ["userinfo-email", "compute-ro", "storage-ro"]
  }
}
```

## Compliant Code Examples
```gcp
#this code is a correct code for which the query should not find any result
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

  service_account {
    email = "email@email.com"
    scopes = ["userinfo-email", "compute-ro", "storage-ro"]
  }
}
```