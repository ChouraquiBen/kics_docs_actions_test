---
title: "Pod or Container Without Security Context"
group-id: "rules/terraform/kubernetes"
meta:
  name: "kubernetes/pod_or_container_without_security_context"
  id: "ad69e38a-d92e-4357-a8da-f2f29d545883"
  display_name: "Pod or Container Without Security Context"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "LOW"
  category: "Insecure Configurations"
---
## Metadata

**Id:** `ad69e38a-d92e-4357-a8da-f2f29d545883`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Low

**Category:** Insecure Configurations

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/pod#security_context)

### Description

 A security context defines privilege and access control settings for a Pod or Container


## Compliant Code Examples
```terraform

resource "kubernetes_pod" "negative4" {
  metadata {
    name = "terraform-example"
  }

  spec {
    security_context = {
        allow_privilege_escalation = false
    }

    container = [
     {
      image = "nginx:1.7.9"
      name  = "example22"

      security_context = {
        allow_privilege_escalation = false
      }

      env = {
        name  = "environment"
        value = "test"
      }

      port = {
        container_port = 8080
      }

      liveness_probe = {
        http_get = {
          path = "/nginx_status"
          port = 80

          http_header = {
            name  = "X-Custom-Header"
            value = "Awesome"
          }
        }

        initial_delay_seconds = 3
        period_seconds        = 3
      }
     }
     ,
     {
      image = "nginx:1.7.9"
      name  = "example22222"

      security_context = {
        allow_privilege_escalation = false
      }

      env = {
        name  = "environment"
        value = "test"
      }

      port = {
        container_port = 8080
      }

      liveness_probe = {
        http_get = {
          path = "/nginx_status"
          port = 80

          http_header = {
            name  = "X-Custom-Header"
            value = "Awesome"
          }
        }

        initial_delay_seconds = 3
        period_seconds        = 3
      }
     }
   ]


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



resource "kubernetes_pod" "negative5" {
  metadata {
    name = "terraform-example"
  }

  spec {
    security_context = {
        allow_privilege_escalation = false
    }

    container {
      image = "nginx:1.7.9"
      name  = "example"

      security_context = {
        allow_privilege_escalation = false
      }

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
resource "kubernetes_pod" "positive1" {
  metadata {
    name = "terraform-example"
  }

  spec {
    container = [
     {
      image = "nginx:1.7.9"
      name  = "example22"

      env = {
        name  = "environment"
        value = "test"
      }

      port = {
        container_port = 8080
      }

      liveness_probe = {
        http_get = {
          path = "/nginx_status"
          port = 80

          http_header = {
            name  = "X-Custom-Header"
            value = "Awesome"
          }
        }

        initial_delay_seconds = 3
        period_seconds        = 3
      }
     }
     ,
     {
      image = "nginx:1.7.9"
      name  = "example22222"

      env = {
        name  = "environment"
        value = "test"
      }

      port = {
        container_port = 8080
      }

      liveness_probe = {
        http_get = {
          path = "/nginx_status"
          port = 80

          http_header = {
            name  = "X-Custom-Header"
            value = "Awesome"
          }
        }

        initial_delay_seconds = 3
        period_seconds        = 3
      }
     }
   ]


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



resource "kubernetes_pod" "positive2" {
  metadata {
    name = "terraform-example"
  }

  spec {
    container {
      image = "nginx:1.7.9"
      name  = "example"

      security_context = {
        allow_privilege_escalation = false
      }

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