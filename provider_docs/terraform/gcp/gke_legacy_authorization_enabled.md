---
title: "GKE Legacy Authorization Enabled"
meta:
  name: "terraform/gke_legacy_authorization_enabled"
  id: "5baa92d2-d8ee-4c75-88a4-52d9d8bb8067"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/gke_legacy_authorization_enabled`
**Id:** `5baa92d2-d8ee-4c75-88a4-52d9d8bb8067`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Insecure Configurations
## Description
Legacy Authorization (ABAC) in GKE grants all authenticated users with full cluster administrator privileges, which violates the principle of least privilege and creates significant security risks. When enabled, any authenticated user can perform any operation on any resource in the cluster, potentially leading to unauthorized access, data breaches, and complete cluster compromise. To secure your GKE cluster, ensure 'enable_legacy_abac' is set to false as shown below:

```hcl
resource "google_container_cluster" "secure_cluster" {
  name               = "marcellus-wallace"
  location           = "us-central1-a"
  initial_node_count = 3
  enable_legacy_abac = false
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/container_cluster)

## Non-Compliant Code Examples
```gcp
#this is a problematic code where the query should report a result(s)
resource "google_container_cluster" "positive1" {
  name               = "marcellus-wallace"
  location           = "us-central1-a"
  initial_node_count = 3
  enable_legacy_abac = true

  timeouts {
    create = "30m"
    update = "40m"
  }
}
```

## Compliant Code Examples
```gcp
#this code is a correct code for which the query should not find any result
resource "google_container_cluster" "negative1" {
  name               = "marcellus-wallace"
  location           = "us-central1-a"
  initial_node_count = 3
  enable_legacy_abac = false

  timeouts {
    create = "30m"
    update = "40m"
  }
}
```