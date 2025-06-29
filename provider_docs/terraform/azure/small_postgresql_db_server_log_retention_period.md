---
title: "Small PostgreSQL DB Server Log Retention Period"
meta:
  name: "terraform/small_postgresql_db_server_log_retention_period"
  id: "261a83f8-dd72-4e8c-b5e1-ebf06e8fe606"
  cloud_provider: "terraform"
  severity: "LOW"
  category: "Observability"
---
## Metadata
**Name:** `terraform/small_postgresql_db_server_log_retention_period`
**Id:** `261a83f8-dd72-4e8c-b5e1-ebf06e8fe606`
**Cloud Provider:** terraform
**Severity:** Low
**Category:** Observability
## Description
This check verifies whether the `log_retention_days` configuration for an Azure PostgreSQL Database Server retains logs for at least 3 days. Insufficient log retention, such as setting `value = 2` in the Terraform resource

```
resource "azurerm_postgresql_configuration" "positive1" {
  name                = "log_retention_days"
  resource_group_name = azurerm_resource_group.example.name
  server_name         = azurerm_postgresql_server.example.name
  value               = 2
}
```

can hinder the ability to investigate security incidents or troubleshoot issues, as critical audit and activity logs may be deleted too quickly. Increasing the retention period to a secure value (such as `value = 5`) helps ensure logs are available for effective monitoring and forensic analysis.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/postgresql_configuration)

## Non-Compliant Code Examples
```azure
resource "azurerm_postgresql_configuration" "positive1" {
  name                = "log_retention_days"
  resource_group_name = azurerm_resource_group.example.name
  server_name         = azurerm_postgresql_server.example.name
  value               = 2
}
```

## Compliant Code Examples
```azure
resource "azurerm_postgresql_configuration" "negative1" {
  name                = "log_retention_days"
  resource_group_name = azurerm_resource_group.example.name
  server_name         = azurerm_postgresql_server.example.name
  value               = 5
}
```