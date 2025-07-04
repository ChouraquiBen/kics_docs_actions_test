---
title: "Default Service Account In Use"
group-id: "rules/terraform/kubernetes"
meta:
  name: "kubernetes/default_service_account_in_use"
  id: "737a0dd9-0aaa-4145-8118-f01778262b8a"
  display_name: "Default Service Account In Use"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "LOW"
  category: "Insecure Configurations"
---
## Metadata

**Id:** `737a0dd9-0aaa-4145-8118-f01778262b8a`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Low

**Category:** Insecure Configurations

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/service_account#automount_service_account_token)

### Description

 Default service accounts should not be actively used


## Compliant Code Examples
```terraform
resource "kubernetes_service_account" "example3" {
  metadata {
    name = "default"
  }

  automount_service_account_token = false
}

```
## Non-Compliant Code Examples
```terraform
resource "kubernetes_service_account" "example" {
  metadata {
    name = "default"
  }
}

resource "kubernetes_service_account" "example2" {
  metadata {
    name = "default"
  }

  automount_service_account_token = true
}

```