---
title: "Cluster Admin Rolebinding With Superuser Permissions"
group-id: "rules/terraform/kubernetes"
meta:
  name: "kubernetes/cluster_admin_role_binding_with_super_user_permissions"
  id: "17172bc2-56fb-4f17-916f-a014147706cd"
  display_name: "Cluster Admin Rolebinding With Superuser Permissions"
  cloud_provider: "kubernetes"
  framework: "Terraform"
  severity: "LOW"
  category: "Access Control"
---
## Metadata

**Id:** `17172bc2-56fb-4f17-916f-a014147706cd`

**Cloud Provider:** kubernetes

**Framework:** Terraform

**Severity:** Low

**Category:** Access Control

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs/resources/cluster_role_binding#name)

### Description

 Ensure that the cluster-admin role is only used where required (RBAC)


## Compliant Code Examples
```terraform
resource "kubernetes_cluster_role_binding" "example1" {
  metadata {
    name = "terraform-example1"
  }
  role_ref {
    api_group = "rbac.authorization.k8s.io"
    kind      = "ClusterRole"
    name      = "cluster"
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
## Non-Compliant Code Examples
```terraform
resource "kubernetes_cluster_role_binding" "example2" {
  metadata {
    name = "terraform-example2"
  }
  role_ref {
    api_group = "rbac.authorization.k8s.io"
    kind      = "ClusterRole"
    name      = "cluster-admin"
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