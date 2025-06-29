---
title: "CosmosDB Account IP Range Filter Not Set"
meta:
  name: "terraform/cosmosdb_account_ip_range_filter_not_set"
  id: "c2a3efb6-8a58-481c-82f2-bfddf34bb4b7"
  cloud_provider: "terraform"
  severity: "CRITICAL"
  category: "Networking and Firewall"
---
## Metadata
**Name:** `terraform/cosmosdb_account_ip_range_filter_not_set`
**Id:** `c2a3efb6-8a58-481c-82f2-bfddf34bb4b7`
**Cloud Provider:** terraform
**Severity:** Critical
**Category:** Networking and Firewall
## Description
Azure CosmosDB Account IP Range Filter provides network-level access control for your database by restricting connections to specified IP addresses or ranges. When this filter is not configured, the database is potentially accessible from any IP address, exposing sensitive data to unauthorized access. Setting the 'ip_range_filter' attribute (e.g., 'ip_range_filter = "104.42.195.92"') limits access to only approved network locations, significantly enhancing your database security posture.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/cosmosdb_account#ip_range_filter)

## Non-Compliant Code Examples
```azure
resource "azurerm_cosmosdb_account" "positive1" {
  name                  = "example" 
  is_virtual_network_filter_enabled = true
 

}
```

## Compliant Code Examples
```azure
resource "azurerm_cosmosdb_account" "negative1" {
  name                  = "example" 

  ip_range_filter       = "104.42.195.92"
  is_virtual_network_filter_enabled = true
 

}
```