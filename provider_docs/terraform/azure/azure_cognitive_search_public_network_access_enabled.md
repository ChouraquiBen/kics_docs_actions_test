---
title: "Azure Cognitive Search Public Network Access Enabled"
meta:
  name: "terraform/azure_cognitive_search_public_network_access_enabled"
  id: "4a9e0f00-0765-4f72-a0d4-d31110b78279"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Networking and Firewall"
---
## Metadata
**Name:** `terraform/azure_cognitive_search_public_network_access_enabled`
**Id:** `4a9e0f00-0765-4f72-a0d4-d31110b78279`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Networking and Firewall
## Description
Allowing public network access to Azure Cognitive Search exposes the service to the internet, increasing the risk of unauthorized access and data exposure. In Terraform, this is controlled by the `public_network_access_enabled` attribute; setting this attribute to `true` permits public connections, while setting it to `false` restricts access to trusted, private networks only. For example, a secure configuration would be:

```
resource "azurerm_search_service" "example" {
  name                          = "example-search-service"
  resource_group_name           = azurerm_resource_group.example.name
  location                      = azurerm_resource_group.example.location
  sku                           = "standard"
  public_network_access_enabled = false
}
```

Leaving public access enabled may allow attackers to enumerate, access, or exfiltrate sensitive search indexes and data.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/search_service#public_network_access_enabled)

## Non-Compliant Code Examples
```azure
resource "azurerm_search_service" "positive1" {
  name                = "example-search-service"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  sku                 = "standard"
  public_network_access_enabled = true
}

```

```azure
resource "azurerm_search_service" "positive2" {
  name                = "example-search-service"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  sku                 = "standard"
}

```

## Compliant Code Examples
```azure
resource "azurerm_search_service" "example" {
  name                = "example-search-service"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  sku                 = "standard"
  public_network_access_enabled = false
}

```