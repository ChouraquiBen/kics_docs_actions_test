---
title: "Deployment Has No PodAntiAffinity"
group-id: "documentation/rules/terraform/kubernetes"
meta:
  name: "kubernetes/deployment_has_no_pod_anti_affinity"
  id: "461ed7e4-f8d5-4bc1-b3c6-64ddb4fd00a3"
  display_name: "Deployment Has No PodAntiAffinity"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "LOW"
  category: "Resource Management"
---
## Metadata

**Id:** `461ed7e4-f8d5-4bc1-b3c6-64ddb4fd00a3`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Low

**Category:** Resource Management

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/deployment#affinity)

### Description

 Check if Deployment resources don't have a podAntiAffinity policy, which prevents multiple pods from being scheduled on the same node.


## Compliant Code Examples
```terraform
resource "kubernetes_deployment" "example433" {
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
        affinity {
          pod_anti_affinity {
            preferred_during_scheduling_ignored_during_execution {
              weight = 100

              pod_affinity_term {
                label_selector {
                  match_labels {
                    k8s-app = "prometheus"
                  }
                }

                topology_key = "kubernetes.io/hostname"
              }
            }
         }
        }

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

```

```terraform
resource "kubernetes_deployment" "example4" {
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
        affinity {
          pod_anti_affinity {
            preferred_during_scheduling_ignored_during_execution {
              weight = 100

              pod_affinity_term {
                label_selector {
                  match_labels {
                    k8s-app = "prometheus2"
                  }
                }

                topology_key = "kubernetes.io/hostname"
              }
            }
         }
        }

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

```terraform
resource "kubernetes_deployment" "example3" {
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
        affinity {
          pod_anti_affinity {
            preferred_during_scheduling_ignored_during_execution {
              weight = 100

              pod_affinity_term {
                label_selector {
                  match_expressions {
                    key      = "security"
                    operator = "In"
                    values   = ["S2"]
                  }
                }

                topology_key = "failure-domain.beta.kubernetes.io/zone"
              }
            }
         }
        }

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