---
title: "Ingress Controller Exposes Workload"
group-id: "rules/terraform/kubernetes"
meta:
  name: "kubernetes/ingress_controller_exposes_workload"
  id: "e2c83c1f-84d7-4467-966c-ed41fd015bb9"
  display_name: "Ingress Controller Exposes Workload"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata

**Id:** `e2c83c1f-84d7-4467-966c-ed41fd015bb9`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Medium

**Category:** Insecure Configurations

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/ingress#http)

### Description

 Ingress Controllers should not expose workload in order to avoid vulnerabilities and DoS attacks


## Compliant Code Examples
```terraform
resource "kubernetes_service" "example" {
  metadata {
    name = "ingress-service"
  }
  spec {
    port {
      port = 80
      target_port = 80
      protocol = "TCP"
    }
    type = "NodePort"
  }
}

resource "kubernetes_ingress" "example" {
  wait_for_load_balancer = true
  metadata {
    name = "example"
    annotations = {
      "kubernetes.io/ingress.class" = "nginx"
    }
  }
  spec {
    rule {
      http {
        path {
          path = "/*"
          backend {
            service_name = kubernetes_service.example.metadata.0.name
            service_port = 80
          }
        }
      }
    }
  }
}

```

```terraform
resource "kubernetes_service" "example-3" {
  metadata {
    name = "ingress-service-3"
  }
  spec {
    port {
      port = 80
      target_port = 80
      protocol = "TCP"
    }
    type = "NodePort"
  }
}

resource "kubernetes_ingress" "example-3" {
  wait_for_load_balancer = true
  metadata {
    name = "example-3"
    annotations = {
      "kubernetes.io/ingress.class" = "nginx"
    }
  }
  spec {
    rule {
      http {
        path {
          path = "/*"
          backend {
            service_name = kubernetes_service.example.metadata.0.name
            service_port = 80
          }
        }
      }
    }
    rule {
      http {
        path {
          path = "/*"
          backend {
            service_name = kubernetes_service.example.metadata.0.name
            service_port = 80
          }
        }
      }
    }
  }
}

```

```terraform
resource "kubernetes_service" "example-2" {
  metadata {
    name = "ingress-service-2"
  }
  spec {
    port {
      port = 80
      target_port = 80
      protocol = "TCP"
    }
    type = "NodePort"
  }
}

resource "kubernetes_ingress" "example-ingress-2" {
  metadata {
    name = "example-ingress"
  }

  spec {
    backend {
      service_name = "MyApp1"
      service_port = 8080
    }

    rule {
      http {
        path {
          backend {
            service_name = "MyApp1"
            service_port = 8080
          }

          path = "/app1/*"
        }

        path {
          backend {
            service_name = "MyApp2"
            service_port = 8080
          }

          path = "/app2/*"
        }
      }
    }

    tls {
      secret_name = "tls-secret"
    }
  }
}

```
## Non-Compliant Code Examples
```terraform
resource "kubernetes_service" "example" {
  metadata {
    name = "ingress-service"
  }
  spec {
    port {
      port = 80
      target_port = 80
      protocol = "TCP"
    }
    type = "NodePort"
  }
}

resource "kubernetes_ingress" "example" {
  wait_for_load_balancer = true
  metadata {
    name = "example"
    annotations = {
      "kubernetes.io/ingress.class" = "nginx"
    }
  }
  spec {
    rule {
      http {
        path {
          path = "/*"
          backend {
            service_name = "example"
            service_port = 80
          }
        }
      }
    }
  }
}

```

```terraform
resource "kubernetes_service" "example-4" {
  metadata {
    name = "ingress-service-4"
  }
  spec {
    port {
      port = 80
      target_port = 80
      protocol = "TCP"
    }
    type = "NodePort"
  }
}

resource "kubernetes_ingress" "example-4" {
  wait_for_load_balancer = true
  metadata {
    name = "example-4"
    annotations = {
      "kubernetes.io/ingress.class" = "nginx"
    }
  }
  spec {
    rule {
      http {
        path {
          path = "/rule1*"
          backend {
            service_name = "example-4"
            service_port = 80
          }
        }
      }
    }
    rule {
      http {
        path {
          path = "/rule2*"
          backend {
            service_name = "service"
            service_port = 80
          }
        }
      }
    }
  }
}

```

```terraform
resource "kubernetes_service" "MyApp2" {
  metadata {
    name = "ingress-service-2"
  }
  spec {
    port {
      port = 80
      target_port = 8080
      protocol = "TCP"
    }
    type = "NodePort"
  }
}

resource "kubernetes_ingress" "example-ingress-2" {
  metadata {
    name = "example-ingress"
    annotations = {
      "kubernetes.io/ingress.class" = "nginx"
    }
  }

  spec {
    backend {
      service_name = "MyApp1"
      service_port = 8080
    }

    rule {
      http {
        path {
          backend {
            service_name = "MyApp1"
            service_port = 8080
          }

          path = "/app1/*"
        }

        path {
          backend {
            service_name = "MyApp2"
            service_port = 8080
          }

          path = "/app2/*"
        }
      }
    }

    tls {
      secret_name = "tls-secret"
    }
  }
}

```