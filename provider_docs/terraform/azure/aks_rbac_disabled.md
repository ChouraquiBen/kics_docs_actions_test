---
title: "AKS RBAC Disabled"
meta:
  name: "terraform/aks_rbac_disabled"
  id: "86f92117-eed8-4614-9c6c-b26da20ff37f"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/aks_rbac_disabled`
**Id:** `86f92117-eed8-4614-9c6c-b26da20ff37f`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Role-Based Access Control (RBAC) should be enabled on Azure Kubernetes Service (AKS) clusters to enforce fine-grained authorization and restrict access to cluster resources. If `role_based_access_control_enabled = false` or `role_based_access_control { enabled = false }` is present in the Terraform configuration, users may gain excessive or unauthorized permissions within the cluster, increasing risk of accidental or malicious actions. Properly configuring RBAC (for example, by using `role_based_access_control_enabled = true`) helps ensure only authorized identities can perform sensitive operations within the AKS environment.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/kubernetes_cluster#role_based_access_control)

## Non-Compliant Code Examples
```azure
resource "azurerm_kubernetes_cluster" "positive1" {
  name                = "example-aks1"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  dns_prefix          = "exampleaks1"

  role_based_access_control_enabled = false

  default_node_pool {
    name       = "default"
    node_count = 1
    vm_size    = "Standard_D2_v2"
  }

  identity {
    type = "SystemAssigned"
  }

  tags = {
    Environment = "Production"
  }

  network_profile {
    network_policy = "azure"
  }
}

resource "azurerm_kubernetes_cluster" "positive2" {
  name                = "example-aks2"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  dns_prefix          = "exampleaks2"

  role_based_access_control {
    enabled = false
  }

  default_node_pool {
    name       = "default"
    node_count = 1
    vm_size    = "Standard_D2_v2"
  }

  identity {
    type = "SystemAssigned"
  }

  tags = {
    Environment = "Production"
  }

  network_profile {
    network_policy = "calico"
  }
}

```

## Compliant Code Examples
```azure
resource "azurerm_kubernetes_cluster" "negative1" {
  name                = "example-aks1"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  dns_prefix          = "exampleaks1"

  role_based_access_control_enabled = true

  default_node_pool {
    name       = "default"
    node_count = 1
    vm_size    = "Standard_D2_v2"
  }

  identity {
    type = "SystemAssigned"
  }

  tags = {
    Environment = "Production"
  }

  network_profile {
    network_policy = "azure"
  }
}


```