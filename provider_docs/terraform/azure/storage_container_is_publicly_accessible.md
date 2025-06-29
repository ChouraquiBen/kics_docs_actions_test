---
title: "Storage Container Is Publicly Accessible"
meta:
  name: "terraform/storage_container_is_publicly_accessible"
  id: "dd5230f8-a577-4bbb-b7ac-f2c2fe7d5299"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/storage_container_is_publicly_accessible`
**Id:** `dd5230f8-a577-4bbb-b7ac-f2c2fe7d5299`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Access Control
## Description
When a storage container in Azure Blob Storage has 'container_access_type' set to 'blob' or 'container', it enables anonymous public access to the data stored within. This configuration creates a significant security risk as it allows anyone with the container URL to access, view, and potentially download the stored data without authentication, potentially leading to sensitive data exposure or unauthorized data access.

To secure your storage containers, always set 'container_access_type' to 'private' explicitly, or rely on the default value which is also 'private'. For example:

```
resource "azurerm_storage_container" "secure_container" {
  name                  = "vhds"
  storage_account_name  = azurerm_storage_account.example.name
  container_access_type = "private"
}
```

Avoid using the insecure configuration shown below:

```
resource "azurerm_storage_container" "insecure_container" {
  name                  = "vhds"
  storage_account_name  = azurerm_storage_account.example.name
  container_access_type = "blob"
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/storage_container#container_access_type)

## Non-Compliant Code Examples
```azure
resource "azurerm_storage_container" "positive1" {
  name                  = "vhds"
  storage_account_name  = azurerm_storage_account.example.name
  container_access_type = "blob"
}
```

## Compliant Code Examples
```azure
resource "azurerm_storage_container" "negative1" {
  name                  = "vhds"
  storage_account_name  = azurerm_storage_account.example.name
  container_access_type = "private"
}

resource "azurerm_storage_container" "negative2" {
  name                  = "vhds2"
  storage_account_name  = azurerm_storage_account.example.name
  // default is "private"
}
```