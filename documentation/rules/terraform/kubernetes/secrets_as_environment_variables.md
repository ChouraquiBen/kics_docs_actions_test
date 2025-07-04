---
title: "Secrets As Environment Variables"
group-id: "documentation/rules/terraform/kubernetes"
meta:
  name: "kubernetes/secrets_as_environment_variables"
  id: "6d8f1a10-b6cd-48f0-b960-f7c535d5cdb8"
  display_name: "Secrets As Environment Variables"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "LOW"
  category: "Secret Management"
---
## Metadata

**Id:** `6d8f1a10-b6cd-48f0-b960-f7c535d5cdb8`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Low

**Category:** Secret Management

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/pod#secret_key_ref)

### Description

 Container should not use secrets as environment variables


## Compliant Code Examples
```terraform
resource "kubernetes_pod" "test55" {
  metadata {
    name = "terraform-example"
  }

  spec {
    container {
      image = "nginx:1.7.9"
      name  = "example"

      env {
        name  = "environment"
        value = "test"
      }

      port {
        container_port = 8080
      }

      liveness_probe {
        http_get {
          path = "/nginx_status"
          port = 80

          http_header {
            name  = "X-Custom-Header"
            value = "Awesome"
          }
        }

        initial_delay_seconds = 3
        period_seconds        = 3
      }
    }

    dns_config {
      nameservers = ["1.1.1.1", "8.8.8.8", "9.9.9.9"]
      searches    = ["example.com"]

      option {
        name  = "ndots"
        value = 1
      }

      option {
        name = "use-vc"
      }
    }

    dns_policy = "None"
  }
}

```
## Non-Compliant Code Examples
```terraform
resource "kubernetes_pod" "test" {
  metadata {
    name = "terraform-example"
  }

  spec {
    container {
      image = "nginx:1.7.9"
      name  = "example"

      env {
        name  = "environment"
        value = "test"

        value_from =  {
            secret_key_ref = "hjjhjh"
        }
      }

      env_from {
        secret_ref = "wwww"
      }

      port {
        container_port = 8080
      }

      liveness_probe {
        http_get {
          path = "/nginx_status"
          port = 80

          http_header {
            name  = "X-Custom-Header"
            value = "Awesome"
          }
        }

        initial_delay_seconds = 3
        period_seconds        = 3
      }
    }

    dns_config {
      nameservers = ["1.1.1.1", "8.8.8.8", "9.9.9.9"]
      searches    = ["example.com"]

      option {
        name  = "ndots"
        value = 1
      }

      option {
        name = "use-vc"
      }
    }

    dns_policy = "None"
  }
}

```