---
title: "Role Binding To Default Service Account"
group-id: "documentation/rules/terraform/kubernetes"
meta:
  name: "kubernetes/role_binding_to_default_service_account"
  id: "3360c01e-c8c0-4812-96a2-a6329b9b7f9f"
  display_name: "Role Binding To Default Service Account"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "MEDIUM"
  category: "Insecure Defaults"
---
## Metadata

**Id:** `3360c01e-c8c0-4812-96a2-a6329b9b7f9f`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Medium

**Category:** Insecure Defaults

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/role_binding#subject)

### Description

 No role nor cluster role should bind to a default service account


## Compliant Code Examples
```terraform
resource "kubernetes_role_binding" "example2" {
  metadata {
    name      = "terraform-example"
    namespace = "default"
  }
  role_ref {
    api_group = "rbac.authorization.k8s.io"
    kind      = "Role"
    name      = "admin"
  }
  subject {
    kind      = "User"
    name      = "admin"
    api_group = "rbac.authorization.k8s.io"
  }
  subject {
    kind      = "ServiceAccount"
    name      = "serviceExample"
    namespace = "kube-system"
  }
  subject {
    kind      = "Group"
    name      = "system:masters"
    api_group = "rbac.authorization.k8s.io"
  }
}

```
## Non-Compliant Code Examples
```terraform
resource "kubernetes_role_binding" "example" {
  metadata {
    name      = "terraform-example"
    namespace = "default"
  }
  role_ref {
    api_group = "rbac.authorization.k8s.io"
    kind      = "Role"
    name      = "admin"
  }
  subject {
    kind      = "User"
    name      = "admin"
    api_group = "rbac.authorization.k8s.io"
  }
  subject {
    kind      = "ServiceAccount"
    name      = "default"
    namespace = "kube-system"
  }
  subject {
    kind      = "Group"
    name      = "system:masters"
    api_group = "rbac.authorization.k8s.io"
  }
}

```