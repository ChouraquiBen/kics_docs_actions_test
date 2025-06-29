---
title: "Google Container Node Pool Auto Repair Disabled"
meta:
  name: "terraform/google_container_node_pool_auto_repair_disabled"
  id: "acfdbec6-4a17-471f-b412-169d77553332"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/google_container_node_pool_auto_repair_disabled`
**Id:** `acfdbec6-4a17-471f-b412-169d77553332`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Configurations
## Description
Enabling auto repair for Google Kubernetes Engine (GKE) node pools ensures that failed or unhealthy nodes are automatically detected and repaired, maintaining cluster health and minimizing manual intervention. If the `auto_repair` attribute is set to `false` or omitted in a Terraform resource like 

```
management {
  auto_repair = false
}
```

unhealthy nodes may persist and degrade application availability or introduce operational risks. To address this, set `auto_repair` to `true` in your Terraform configuration:

```
management {
  auto_repair = true
}
```

This configuration helps maintain a resilient and self-healing node environment in your GKE cluster.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/container_node_pool)

## Non-Compliant Code Examples
```gcp
resource "google_container_cluster" "positive1" {
  name     = "my-gke-cluster"
  location = "us-central1"
  remove_default_node_pool = true
  initial_node_count       = 1
}

resource "google_container_node_pool" "positive2" {
  name       = "my-node-pool"
  location   = "us-central1"
  cluster    = google_container_cluster.primary.name
  node_count = 1

  management {
    auto_repair  = false
  }
}

resource "google_container_node_pool" "positive3" {
  name       = "my-node-pool"
  location   = "us-central1"
  cluster    = google_container_cluster.primary.name
  node_count = 1
}
```

## Compliant Code Examples
```gcp
resource "google_container_cluster" "negative1" {
  name     = "my-gke-cluster"
  location = "us-central1"
  remove_default_node_pool = true
  initial_node_count       = 1
}

resource "google_container_node_pool" "negative2" {
  name       = "my-node-pool"
  location   = "us-central1"
  cluster    = google_container_cluster.primary.name
  node_count = 1

  management {
    auto_repair  = true
  }
}

```