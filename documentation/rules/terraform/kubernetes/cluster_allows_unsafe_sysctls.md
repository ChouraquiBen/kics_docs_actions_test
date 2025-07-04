---
title: "Cluster Allows Unsafe Sysctls"
group-id: "documentation/rules/terraform/kubernetes"
meta:
  name: "kubernetes/cluster_allows_unsafe_sysctls"
  id: "a9174d31-d526-4ad9-ace4-ce7ddbf52e03"
  display_name: "Cluster Allows Unsafe Sysctls"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "HIGH"
  category: "Insecure Configurations"
---
## Metadata

**Id:** `a9174d31-d526-4ad9-ace4-ce7ddbf52e03`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** High

**Category:** Insecure Configurations

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/pod_security_policy#allowed_unsafe_sysctls)

### Description

 A Kubernetes Cluster must not allow unsafe sysctls, to prevent a pod from having any influence on any other pod on the node, harming the node's health or gaining CPU or memory resources outside of the resource limits of a pod. This means the 'spec.security_context.sysctl' must not have an unsafe sysctls and that the attribute 'allowed_unsafe_sysctls' must be undefined.


## Compliant Code Examples
```terraform
resource "kubernetes_pod_security_policy" "exampleW" {
  metadata {
    name = "terraform-example"
  }
  spec {
    privileged                 = false
    allow_privilege_escalation = false

    volumes = [
      "configMap",
      "emptyDir",
      "projected",
      "secret",
      "downwardAPI",
      "persistentVolumeClaim",
    ]

    run_as_user {
      rule = "MustRunAsNonRoot"
    }

    se_linux {
      rule = "RunAsAny"
    }

    supplemental_groups {
      rule = "MustRunAs"
      range {
        min = 1
        max = 65535
      }
    }

    fs_group {
      rule = "MustRunAs"
      range {
        min = 1
        max = 65535
      }
    }

    read_only_root_filesystem = true
  }
}


```

```terraform

resource "kubernetes_pod" "test2" {
  metadata {
    name = "terraform-example"
  }

  spec {
    security_context {
      sysctl = [
        {
          name = "kernel.shm_rmid_forced"
          value = "0"
        }
      ]
    }
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
resource "kubernetes_pod_security_policy" "example" {
  metadata {
    name = "terraform-example"
  }
  spec {
    allowed_unsafe_sysctls = ["kernel.msg*"]
    privileged                 = false
    allow_privilege_escalation = false

    volumes = [
      "configMap",
      "emptyDir",
      "projected",
      "secret",
      "downwardAPI",
      "persistentVolumeClaim",
    ]

    run_as_user {
      rule = "MustRunAsNonRoot"
    }

    se_linux {
      rule = "RunAsAny"
    }

    supplemental_groups {
      rule = "MustRunAs"
      range {
        min = 1
        max = 65535
      }
    }

    fs_group {
      rule = "MustRunAs"
      range {
        min = 1
        max = 65535
      }
    }

    read_only_root_filesystem = true
  }
}


```

```terraform

resource "kubernetes_pod" "test" {
  metadata {
    name = "terraform-example"
  }

  spec {
    security_context {
      sysctl = [
        {
          name = "net.core.somaxconn"
          value = "1024"
        }
      ]
    }
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