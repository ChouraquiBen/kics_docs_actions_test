---
title: "Role Assignment Not Limit Guest User Permissions"
meta:
  name: "terraform/role_assignment_not_limit_guest_users_permissions"
  id: "8e75e431-449f-49e9-b56a-c8f1378025cf"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/role_assignment_not_limit_guest_users_permissions`
**Id:** `8e75e431-449f-49e9-b56a-c8f1378025cf`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Role assignments in Terraform should strictly limit permissions granted to guest users. If the `actions` attribute in the `azurerm_role_definition` resource is set to `["*"]`, guest users receive unrestricted permissions within the scope, potentially allowing them to perform any action, escalate privileges, or exfiltrate data. It is recommended to set `actions = []` and `not_actions = ["*"]` to ensure that guest users have no actionable privileges, thereby protecting critical resources from unauthorized access.

```
permissions {
  actions     = []
  not_actions = ["*"]
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/role_assignment)

## Non-Compliant Code Examples
```azure
resource "azurerm_role_definition" "example" {
  name        = "my-custom-role"
  scope       = data.azurerm_subscription.primary.id
  description = "This is a custom role created via Terraform"

  permissions {
    actions     = ["*"]
    not_actions = []
  }

  assignable_scopes = [
    data.azurerm_subscription.primary.id, 
  ]
}

resource "azurerm_role_assignment" "example" {
  name               = "00000000-0000-0000-0000-000000000000"
  scope              = data.azurerm_subscription.primary.id
  role_definition_name = "Guest"
  role_definition_id = azurerm_role_definition.example.role_definition_resource_id
  principal_id       = data.azurerm_client_config.example.object_id
}

```

## Compliant Code Examples
```azure
resource "azurerm_role_definition" "example2" {
  name        = "my-custom-role"
  scope       = data.azurerm_subscription.primary.id
  description = "This is a custom role created via Terraform"

  permissions {
    actions     = []
    not_actions = ["*"]
  }

  assignable_scopes = [
    data.azurerm_subscription.primary.id, 
  ]
}

resource "azurerm_role_assignment" "example2" {
  name               = "00000000-0000-0000-0000-000000000000"
  scope              = data.azurerm_subscription.primary.id
  role_definition_name = "Guest"
  role_definition_id = azurerm_role_definition.example2.role_definition_resource_id
  principal_id       = data.azurerm_client_config.example.object_id
}

```