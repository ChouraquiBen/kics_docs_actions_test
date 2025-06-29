---
title: "Dataproc Clusters Has Public IPs"
meta:
  name: "terraform/dataproc_cluster_has_public_ip"
  id: "d2c4b6a8-1234-4f56-9abc-def012345678"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/dataproc_cluster_has_public_ip`
**Id:** `d2c4b6a8-1234-4f56-9abc-def012345678`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Insecure Configurations
## Description
Google Cloud Dataproc clusters with public IP addresses are directly accessible from the internet, creating an expanded attack surface that could be exploited by malicious actors. When 'internal_ip_only' is set to false or omitted, clusters receive both internal and external IP addresses, potentially exposing sensitive data processing operations and administrative interfaces to unauthorized access.

Secure configuration requires setting 'internal_ip_only' to true as shown in this example:
```terraform
resource "google_dataproc_cluster" "good_example" {
  cluster_config {
    gce_cluster_config {
      internal_ip_only = true
    }
  }
}
```

Insecure configuration that should be avoided:
```terraform
resource "google_dataproc_cluster" "bad_example" {
  cluster_config {
    gce_cluster_config {
      internal_ip_only = false
    }
  }
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/dataproc_cluster)

## Non-Compliant Code Examples
```gcp
resource "google_dataproc_cluster" "bad_example" {
  name   = "bad-cluster"
  region = "us-central1"

  cluster_config {
    gce_cluster_config {
      internal_ip_only = false # ❌ Public IP enabled
    }
  }
}

```

## Compliant Code Examples
```gcp
resource "google_dataproc_cluster" "good_example" {
  name   = "good-cluster"
  region = "us-central1"

  cluster_config {
    gce_cluster_config {
      internal_ip_only = true # ✅ Private cluster (no public IP)
    }
  }
}

```