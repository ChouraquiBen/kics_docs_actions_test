---
title: "MSSQL Server Auditing Disabled"
meta:
  name: "terraform/mssql_server_auditing_disabled"
  id: "609839ae-bd81-4375-9910-5bce72ae7b92"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Observability"
---
## Metadata
**Name:** `terraform/mssql_server_auditing_disabled`
**Id:** `609839ae-bd81-4375-9910-5bce72ae7b92`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Observability
## Description
Auditing should be enabled for MSSQL servers to ensure that access and activity logs are generated and retained for security monitoring and incident response. Without the `azurerm_mssql_server_extended_auditing_policy` resource configured, suspicious activities or potential breaches may go undetected, increasing the risk of data compromise. Ensuring auditing is on can be achieved by adding a resource like:

```
resource "azurerm_mssql_server_extended_auditing_policy" "example" {
  database_id      = azurerm_mssql_database.example.id
  storage_endpoint = azurerm_storage_account.example.primary_blob_endpoint
  retention_in_days = 6
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/mssql_server)

## Non-Compliant Code Examples
```azure
resource "azurerm_mssql_server" "positive1" {
    name                         = "mssqlserver"
    resource_group_name          = azurerm_resource_group.example.name
    location                     = azurerm_resource_group.example.location
    version                      = "12.0"
    administrator_login          = "mradministrator"
    administrator_login_password = "thisIsDog11"
}
```

## Compliant Code Examples
```azure
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West Europe"
}

resource "azurerm_mssql_server" "example" {
    name                         = "mssqlserver"
    resource_group_name          = azurerm_resource_group.example.name
    location                     = azurerm_resource_group.example.location
    version                      = "12.0"
    administrator_login          = "mradministrator"
    administrator_login_password = "thisIsDog11"
}

resource "azurerm_mssql_database" "example" {
  name      = "example-db"
  server_id = azurerm_mssql_server.example.id
}

resource "azurerm_storage_account" "example" {
  name                     = "examplesa"
  resource_group_name      = azurerm_resource_group.example.name
  location                 = azurerm_resource_group.example.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_mssql_server_extended_auditing_policy" "example" {
  database_id                             = azurerm_mssql_database.example.id
  storage_endpoint                        = azurerm_storage_account.example.primary_blob_endpoint
  storage_account_access_key              = azurerm_storage_account.example.primary_access_key
  storage_account_access_key_is_secondary = false
  retention_in_days                       = 6
}

```