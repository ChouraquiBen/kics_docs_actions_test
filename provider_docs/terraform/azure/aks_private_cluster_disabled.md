---
title: "AKS Private Cluster Disabled"
meta:
  name: "terraform/aks_private_cluster_disabled"
  id: "599318f2-6653-4569-9e21-041d06c63a89"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/aks_private_cluster_disabled`
**Id:** `599318f2-6653-4569-9e21-041d06c63a89`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Configurations
## Description
The Azure Kubernetes Service (AKS) API server should not be exposed directly to the internet, as this increases the risk of unauthorized access and potential exploitation of the cluster. When the `private_cluster_enabled` attribute is set to `false`, as shown below, the AKS API endpoint is accessible publicly, allowing threat actors to attempt brute force or other attacks:

```
resource "azurerm_kubernetes_cluster" "example" {
  // ...
  private_cluster_enabled = false
}
```

To mitigate this risk, the attribute should be set to `true`, ensuring the API server is only accessible from internal networks and reducing the attack surface:

```
resource "azurerm_kubernetes_cluster" "example" {
  // ...
  private_cluster_enabled = true
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/kubernetes_cluster#private_cluster_enabled)

## Non-Compliant Code Examples
```azure
resource "azurerm_kubernetes_cluster" "positive1" {
  name                = "example-aks1"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  dns_prefix          = "exampleaks1"

  private_cluster_enabled = false
}

```

```azure
resource "azurerm_kubernetes_cluster" "positive2" {
  name                = "example-aks1"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  dns_prefix          = "exampleaks1"

}

```

## Compliant Code Examples
```azure
resource "azurerm_kubernetes_cluster" "negative" {
  name                = "example-aks1"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  dns_prefix          = "exampleaks1"

  private_cluster_enabled = true
}

```