---
title: "Shared Service Account"
group-id: "rules/terraform/kubernetes"
meta:
  name: "kubernetes/shared_service_account"
  id: "f74b9c43-161a-4799-bc95-0b0ec81801b9"
  display_name: "Shared Service Account"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "MEDIUM"
  category: "Secret Management"
---
## Metadata

**Id:** `f74b9c43-161a-4799-bc95-0b0ec81801b9`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Medium

**Category:** Secret Management

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/pod#service_account_name)

### Description

 A Service Account token is shared between workloads


## Compliant Code Examples
```terraform
resource "kubernetes_pod" "with_pod_affinity_1" {
  metadata {
    name = "with-pod-affinity-1"
  }

  spec {
    affinity {
      pod_affinity {
        required_during_scheduling_ignored_during_execution {
          label_selector {
            match_expressions {
              key      = "security"
              operator = "In"
              values   = ["S1"]
            }
          }

          topology_key = "failure-domain.beta.kubernetes.io/zone"
        }
      }

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
      name  = "with-pod-affinity"
      image = "k8s.gcr.io/pause:2.0"
    }
  }
}

resource "kubernetes_pod" "with_pod_affinity_2" {
  metadata {
    name = "with-pod-affinity-2"
  }

  spec {
    affinity {
      pod_affinity {
        required_during_scheduling_ignored_during_execution {
          label_selector {
            match_expressions {
              key      = "security"
              operator = "In"
              values   = ["S1"]
            }
          }

          topology_key = "failure-domain.beta.kubernetes.io/zone"
        }
      }

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
      name  = "with-pod-affinity"
      image = "k8s.gcr.io/pause:2.0"
    }

    service_account_name = "service-name"
  }
}

resource "kubernetes_service_account" "example" {
  metadata {
    name = "example"
  }
  secret {
    name = kubernetes_secret.example.metadata.0.name
  }
}

resource "kubernetes_secret" "example" {
  metadata {
    name = "terraform-example"
  }
}

```
## Non-Compliant Code Examples
```terraform
resource "kubernetes_pod" "with_pod_affinity" {
  metadata {
    name = "with-pod-affinity"
  }

  spec {
    affinity {
      pod_affinity {
        required_during_scheduling_ignored_during_execution {
          label_selector {
            match_expressions {
              key      = "security"
              operator = "In"
              values   = ["S1"]
            }
          }

          topology_key = "failure-domain.beta.kubernetes.io/zone"
        }
      }

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
      name  = "with-pod-affinity"
      image = "k8s.gcr.io/pause:2.0"
    }

    service_account_name = "terraform-example"
  }
}

resource "kubernetes_service_account" "terraform-example" {
  metadata {
    name = "terraform-example"
  }
  secret {
    name = kubernetes_secret.example.metadata.0.name
  }
}

resource "kubernetes_secret" "example" {
  metadata {
    name = "terraform-example"
  }
}

```