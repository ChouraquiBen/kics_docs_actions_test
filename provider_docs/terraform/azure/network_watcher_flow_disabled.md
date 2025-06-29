---
title: "Network Watcher Flow Disabled"
meta:
  name: "terraform/network_watcher_flow_disabled"
  id: "b90842e5-6779-44d4-9760-972f4c03ba1c"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/network_watcher_flow_disabled`
**Id:** `b90842e5-6779-44d4-9760-972f4c03ba1c`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Configurations
## Description
This check ensures that the `enabled` attribute in the `azurerm_network_watcher_flow_log` resource is set to `true`, which activates flow logging for the associated network security group. Disabling flow logs by setting `enabled = false` can result in a lack of visibility into network traffic, making it difficult to detect and investigate security incidents and unauthorized access attempts in Azure environments. To maintain proper monitoring and auditing, the flow log should be enabled as shown below:

```
resource "azurerm_network_watcher_flow_log" "secure_example" {
  network_watcher_name       = azurerm_network_watcher.test.name
  resource_group_name        = azurerm_resource_group.test.name
  network_security_group_id  = azurerm_network_security_group.test.id
  storage_account_id         = azurerm_storage_account.test.id
  enabled                    = true

  retention_policy {
    enabled = true
    days    = 7
  }
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_watcher_flow_log)

## Non-Compliant Code Examples
```azure
resource "azurerm_network_watcher_flow_log" "positive1" {
  network_watcher_name = azurerm_network_watcher.test.name
  resource_group_name  = azurerm_resource_group.test.name

  network_security_group_id = azurerm_network_security_group.test.id
  storage_account_id        = azurerm_storage_account.test.id
  enabled                   = false

  retention_policy {
    enabled = true
    days    = 7
  }
}
```

## Compliant Code Examples
```azure
resource "azurerm_network_watcher_flow_log" "negative1" {
  network_watcher_name = azurerm_network_watcher.test.name
  resource_group_name  = azurerm_resource_group.test.name

  network_security_group_id = azurerm_network_security_group.test.id
  storage_account_id        = azurerm_storage_account.test.id
  enabled                   = true

  retention_policy {
    enabled = true
    days    = 7
  }
}
```