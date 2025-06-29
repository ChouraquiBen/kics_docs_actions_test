---
title: "MariaDB Server Geo-redundant Backup Disabled"
meta:
  name: "terraform/mariadb_server_georedundant_backup_disabled"
  id: "0a70d5f3-1ecd-4c8e-9292-928fc9a8c4f1"
  cloud_provider: "terraform"
  severity: "LOW"
  category: "Backup"
---
## Metadata
**Name:** `terraform/mariadb_server_georedundant_backup_disabled`
**Id:** `0a70d5f3-1ecd-4c8e-9292-928fc9a8c4f1`
**Cloud Provider:** terraform
**Severity:** Low
**Category:** Backup
## Description
MariaDB Servers deployed in Azure should have geo-redundant backup enabled by setting the `geo_redundant_backup_enabled` attribute to `true`. Without geo-redundant backups, your database backups are stored only within the primary region, making them vulnerable to regional outages or disasters that could lead to data loss. Enabling geo-redundant backups ensures that your backups are replicated to a paired Azure region and increases your ability to restore data in the event of a regional failure.

```hcl
resource "azurerm_mariadb_server" "secure_example" {
  // ... other configuration ...
  geo_redundant_backup_enabled = true
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/mariadb_server#geo_redundant_backup_enabled)

## Non-Compliant Code Examples
```azure
resource "azurerm_mariadb_server" "positive1" {
  name                = "example-mariadb-server"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  administrator_login          = "mariadbadmin"
  administrator_login_password = "H@Sh1CoR3!"

  sku_name   = "B_Gen5_2"
  storage_mb = 5120
  version    = "10.2"

  auto_grow_enabled             = true
  backup_retention_days         = 7
  geo_redundant_backup_enabled  = false
  public_network_access_enabled = false
  ssl_enforcement_enabled       = true
}

```

```azure
resource "azurerm_mariadb_server" "positive2" {
  name                = "example-mariadb-server"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  administrator_login          = "mariadbadmin"
  administrator_login_password = "H@Sh1CoR3!"

  sku_name   = "B_Gen5_2"
  storage_mb = 5120
  version    = "10.2"

  auto_grow_enabled             = true
  backup_retention_days         = 7
  public_network_access_enabled = false
  ssl_enforcement_enabled       = true
}

```

## Compliant Code Examples
```azure
resource "azurerm_mariadb_server" "negative" {
  name                = "example-mariadb-server"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  administrator_login          = "mariadbadmin"
  administrator_login_password = "H@Sh1CoR3!"

  sku_name   = "B_Gen5_2"
  storage_mb = 5120
  version    = "10.2"

  auto_grow_enabled             = true
  backup_retention_days         = 7
  geo_redundant_backup_enabled  = true
  public_network_access_enabled = false
  ssl_enforcement_enabled       = true
}

```