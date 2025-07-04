---
title: "Deployment Without PodDisruptionBudget"
group-id: "documentation/rules/terraform/kubernetes"
meta:
  name: "kubernetes/deployment_without_pod_disruption_budget"
  id: "a05331ee-1653-45cb-91e6-13637a76e4f0"
  display_name: "Deployment Without PodDisruptionBudget"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "LOW"
  category: "Availability"
---
## Metadata

**Id:** `a05331ee-1653-45cb-91e6-13637a76e4f0`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Low

**Category:** Availability

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/deployment#selector)

### Description

 Deployments should be assigned with a PodDisruptionBudget to ensure high availability


## Compliant Code Examples
```terraform
resource "kubernetes_deployment" "example2" {
  metadata {
    name = "terraform-example"
    labels = {
      k8s-app2 = "prometheus2"
    }
  }

  spec {
    replicas = 3

    selector {
      match_labels = {
        k8s-app2 = "prometheus2"
      }
    }

    template {
      metadata {
        labels = {
          k8s-app2 = "prometheus2"
        }
      }

      spec {
        container {
          image = "nginx:1.7.8"
          name  = "example"

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
      }
    }
  }
}


resource "kubernetes_pod_disruption_budget" "demo2" {
  metadata {
    name = "demo"
  }
  spec {
    max_unavailable = "20%"
    selector {
      match_labels = {
        k8s-app2 = "prometheus2"
      }
    }
  }
}



resource "kubernetes_deployment" "example3" {
  metadata {
    name = "terraform-example"
    labels = {
      k8s-app2 = "prometheus2"
    }
  }

  spec {
    replicas = 3

    selector {
      match_labels = {
        k8s-app2 = "kubernetes_pod_disruption_budget.demo2.spec.selector.0.match_labels.k8s-app2"
      }
    }

    template {
      metadata {
        labels = {
          k8s-app2 = "prometheus2"
        }
      }

      spec {
        container {
          image = "nginx:1.7.8"
          name  = "example"

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
      }
    }
  }
}

```
## Non-Compliant Code Examples
```terraform
resource "kubernetes_deployment" "example" {
  metadata {
    name = "terraform-example"
    labels = {
      k8s-app = "prometheus"
    }
  }

  spec {
    replicas = 3

    selector {
      match_labels = {
        k8s-app = "prometheus"
      }
    }

    template {
      metadata {
        labels = {
          k8s-app = "prometheus"
        }
      }

      spec {
        container {
          image = "nginx:1.7.8"
          name  = "example"

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
      }
    }
  }
}


resource "kubernetes_pod_disruption_budget" "demo" {
  metadata {
    name = "demo"
  }
  spec {
    max_unavailable = "20%"
    selector {
      match_labels = {
        test = "MyExampleApp"
      }
    }
  }
}

```