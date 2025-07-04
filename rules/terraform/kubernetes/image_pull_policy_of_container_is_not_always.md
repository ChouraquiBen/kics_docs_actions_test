---
title: "Image Pull Policy Of The Container Is Not Set To Always"
group-id: "rules/terraform/kubernetes"
meta:
  name: "kubernetes/image_pull_policy_of_container_is_not_always"
  id: "aa737abf-6b1d-4aba-95aa-5c160bd7f96e"
  display_name: "Image Pull Policy Of The Container Is Not Set To Always"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "LOW"
  category: "Insecure Configurations"
---
## Metadata

**Id:** `aa737abf-6b1d-4aba-95aa-5c160bd7f96e`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Low

**Category:** Insecure Configurations

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/pod#image_pull_policy)

### Description

 Image Pull Policy of the container must be defined and set to Always


## Compliant Code Examples
```terraform
resource "kubernetes_pod" "busybox" {
  metadata {
    name = "busybox-tf"
  }

  spec {
    container {
      image   = "busybox"
      command = ["sleep", "3600"]
      name    = "busybox"

      image_pull_policy = "Always"
    }

    restart_policy = "Always"
  }
}

```
## Non-Compliant Code Examples
```terraform
resource "kubernetes_pod" "busybox" {
  metadata {
    name = "busybox-tf"
  }

  spec {
    container {
      image   = "busybox"
      command = ["sleep", "3600"]
      name    = "busybox"

      image_pull_policy = "IfNotPresent"
    }

    restart_policy = "Always"
  }
}

```

```terraform

resource "kubernetes_deployment" "example" {
  metadata {
    name = "terraform-example"
    labels = {
      test = "MyExampleApp"
    }
  }

  spec {
    replicas = 3

    selector {
      match_labels = {
        test = "MyExampleApp"
      }
    }

    template {
      metadata {
        labels = {
          test = "MyExampleApp"
        }
      }

      spec {
        container {
          image             = "nginx:1.7.8"
          name              = "example"
          image_pull_policy = "IfNotPresent"

          resources {
            limits = {
              cpu    = "0.5"
              memory = "512Mi"
            }
            requests = {
              cpu    = "250m"
              memory = "50Mi"
            }
          }
        }
      }
    }
  }
}

```