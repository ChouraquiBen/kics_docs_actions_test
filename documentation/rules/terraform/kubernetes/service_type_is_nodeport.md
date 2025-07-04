---
title: "Service Type is NodePort"
group-id: "documentation/rules/terraform/kubernetes"
meta:
  name: "kubernetes/service_type_is_nodeport"
  id: "5c281bf8-d9bb-47f2-b909-3f6bb11874ad"
  display_name: "Service Type is NodePort"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "LOW"
  category: "Networking and Firewall"
---
## Metadata

**Id:** `5c281bf8-d9bb-47f2-b909-3f6bb11874ad`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Low

**Category:** Networking and Firewall

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/service#type)

### Description

 Service type should not be NodePort


## Compliant Code Examples
```terraform
resource "kubernetes_service" "example2" {
  metadata {
    name = "terraform-example"
  }
  spec {
    selector = {
      app = kubernetes_pod.example.metadata.0.labels.app
    }
    session_affinity = "ClientIP"
    port {
      port        = 8080
      target_port = 80
    }

    type = "LoadBalancer"
  }
}

```
## Non-Compliant Code Examples
```terraform
resource "kubernetes_service" "example" {
  metadata {
    name = "terraform-example"
  }
  spec {
    selector = {
      app = kubernetes_pod.example.metadata.0.labels.app
    }
    session_affinity = "ClientIP"
    port {
      port        = 8080
      target_port = 80
    }

    type = "NodePort"
  }
}

```