---
title: "Function App Managed Identity Disabled"
meta:
  name: "terraform/function_app_managed_identity_disabled"
  id: "c87749b3-ff10-41f5-9df2-c421e8151759"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/function_app_managed_identity_disabled`
**Id:** `c87749b3-ff10-41f5-9df2-c421e8151759`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Configurations
## Description
Azure Function Apps should have managed identities enabled to allow for secure authentication to Azure services without the need for hard-coded credentials. If the `identity` block is omitted in a Terraform resource, as in the example below, the Function App will not have a managed identity and may rely on less secure methods such as embedding credentials in code or configuration:

```
resource "azurerm_function_app" "insecure" {
  name                       = "test-azure-functions"
  location                   = azurerm_resource_group.example.location
  resource_group_name        = azurerm_resource_group.example.name
  app_service_plan_id        = azurerm_app_service_plan.example.id
  storage_account_name       = azurerm_storage_account.example.name
  storage_account_access_key = azurerm_storage_account.example.primary_access_key
}
```

Enabling a managed identity using the `identity { type = "SystemAssigned" }` block in your configuration ensures secure service-to-service communication and reduces the risk of credential leakage:

```
resource "azurerm_function_app" "secure" {
  name                       = "test-azure-functions"
  location                   = azurerm_resource_group.example.location
  resource_group_name        = azurerm_resource_group.example.name
  app_service_plan_id        = azurerm_app_service_plan.example.id
  storage_account_name       = azurerm_storage_account.example.name
  storage_account_access_key = azurerm_storage_account.example.primary_access_key

  identity {
    type = "SystemAssigned"
  }
}
```

Leaving this unaddressed may expose sensitive data or allow unauthorized access to connected Azure resources through weaker authentication mechanisms.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/function_app#identity)

## Non-Compliant Code Examples
```azure
resource "azurerm_function_app" "positive1" {
  name                       = "test-azure-functions"
  location                   = azurerm_resource_group.example.location
  resource_group_name        = azurerm_resource_group.example.name
  app_service_plan_id        = azurerm_app_service_plan.example.id
  storage_account_name       = azurerm_storage_account.example.name
  storage_account_access_key = azurerm_storage_account.example.primary_access_key
}

```

## Compliant Code Examples
```azure
resource "azurerm_function_app" "negative" {
  name                       = "test-azure-functions"
  location                   = azurerm_resource_group.example.location
  resource_group_name        = azurerm_resource_group.example.name
  app_service_plan_id        = azurerm_app_service_plan.example.id
  storage_account_name       = azurerm_storage_account.example.name
  storage_account_access_key = azurerm_storage_account.example.primary_access_key

  identity {
    type = "SystemAssigned"
  }
}

```