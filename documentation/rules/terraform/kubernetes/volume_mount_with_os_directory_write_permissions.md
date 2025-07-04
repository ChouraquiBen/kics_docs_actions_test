---
title: "Volume Mount With OS Directory Write Permissions"
group-id: "documentation/rules/terraform/kubernetes"
meta:
  name: "kubernetes/volume_mount_with_os_directory_write_permissions"
  id: "a62a99d1-8196-432f-8f80-3c100b05d62a"
  display_name: "Volume Mount With OS Directory Write Permissions"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "HIGH"
  category: "Resource Management"
---
## Metadata

**Id:** `a62a99d1-8196-432f-8f80-3c100b05d62a`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** High

**Category:** Resource Management

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/pod#volume_mount)

### Description

 Containers can mount sensitive folders from the hosts, giving them potentially dangerous access to critical host configurations and binaries.


## Compliant Code Examples
```terraform
resource "kubernetes_pod" "testttt" {
  metadata {
    name = "terraform-example"
  }

  spec {
    container {
      volume_mount {
        name       = "config-volume"
        mount_path = "/etc/config"
        read_only  = true
      }

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
      volume_mount {
        name       = "config-volume"
        mount_path = "/bin"
      }

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

```terraform
resource "kubernetes_pod" "test3" {
  metadata {
    name = "terraform-example"
  }

  spec {
    container {
      volume_mount = [
        {
          name       = "config-volume"
          mount_path = "/bin"
          read_only = false
        }

      ]

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

```terraform
resource "kubernetes_pod" "test2" {
  metadata {
    name = "terraform-example"
  }

  spec {
    container {
      volume_mount {
        name       = "config-volume"
        mount_path = "/bin"
        read_only = false
      }

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