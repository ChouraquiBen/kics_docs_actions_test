---
title: "MySQL SSL Connection Disabled"
meta:
  name: "terraform/mysql_ssl_connection_disabled"
  id: "73e42469-3a86-4f39-ad78-098f325b4e9f"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Encryption"
---
## Metadata
**Name:** `terraform/mysql_ssl_connection_disabled`
**Id:** `73e42469-3a86-4f39-ad78-098f325b4e9f`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Encryption
## Description
To ensure data transmitted between clients and the MySQL server is secure, the `ssl_enforcement_enabled` attribute in the `azurerm_mysql_server` resource should be set to `true`. If `ssl_enforcement_enabled` is set to `false`, as shown below, database connections can occur over unencrypted channels, potentially exposing sensitive information such as credentials and application data to interception and misuse.

```
resource "azurerm_mysql_server" "example" {
  ...
  ssl_enforcement_enabled = false
}
```

Enabling SSL enforcement mitigates this risk by ensuring that all clients must connect using SSL, protecting data in transit.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/mysql_server)

## Non-Compliant Code Examples
```azure
resource "azurerm_mysql_server" "positive1" {
  name                = "webflux-mysql-${var.environment}${random_integer.rnd_int.result}"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  administrator_login          = "webflux-${var.environment}"
  administrator_login_password = random_string.password.result

  sku_name   = "B_Gen5_2"
  storage_mb = 5120
  version    = "5.7"

  auto_grow_enabled                 = true
  backup_retention_days             = 7
  infrastructure_encryption_enabled = true
  public_network_access_enabled     = true
  ssl_enforcement_enabled           = false
}

```

## Compliant Code Examples
```azure
resource "azurerm_mysql_server" "negative1" {
  name                = "webflux-mysql-${var.environment}${random_integer.rnd_int.result}"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  administrator_login          = "webflux-${var.environment}"
  administrator_login_password = random_string.password.result

  sku_name   = "B_Gen5_2"
  storage_mb = 5120
  version    = "5.7"

  auto_grow_enabled                 = true
  backup_retention_days             = 7
  infrastructure_encryption_enabled = true
  public_network_access_enabled     = true
  ssl_enforcement_enabled           = true
}

```