---
title: "Using Default Namespace"
group-id: "rules/terraform/kubernetes"
meta:
  name: "kubernetes/using_default_namespace"
  id: "abcb818b-5af7-4d72-aba9-6dd84956b451"
  display_name: "Using Default Namespace"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "LOW"
  category: "Insecure Configurations"
---
## Metadata

**Id:** `abcb818b-5af7-4d72-aba9-6dd84956b451`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Low

**Category:** Insecure Configurations

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/pod#namespace)

### Description

 The default namespace should not be used


## Compliant Code Examples
```terraform
resource "kubernetes_pod" "test3" {
  metadata {
    name = "terraform-example"
    namespace = "terraform-namespace"
  }
}

resource "kubernetes_cron_job" "test4" {
  metadata {
    name = "terraform-example"
    namespace = "terraform-namespace"
  }
}

```
## Non-Compliant Code Examples
```terraform
resource "kubernetes_pod" "test" {
  metadata {
    name = "terraform-example"
    namespace = "default"
  }
}

resource "kubernetes_cron_job" "test2" {
  metadata {
    name = "terraform-example"
  }
}

```