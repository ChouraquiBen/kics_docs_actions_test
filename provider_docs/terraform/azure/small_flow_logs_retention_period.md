---
title: "Small Flow Logs Retention Period"
meta:
  name: "terraform/small_flow_logs_retention_period"
  id: "7750fcca-dd03-4d38-b663-4b70289bcfd4"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/small_flow_logs_retention_period`
**Id:** `7750fcca-dd03-4d38-b663-4b70289bcfd4`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Configurations
## Description
Network Security Group Flow Logs capture critical information about IP traffic flowing in and out of network security groups, aiding in the detection of anomalies and potential security breaches. If the flow logs are not retained for at least 90 days—for example, using a Terraform configuration where `retention_policy { days = 3 }`—important forensic data could be lost, making it difficult to investigate incidents or compromise attempts. Ensuring the attribute is set as shown below helps maintain compliance and enables sufficient investigation time:

```
retention_policy {
  enabled = true
  days    = 90
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_watcher_flow_log)

## Non-Compliant Code Examples
```azure
resource "azurerm_network_watcher_flow_log" "positive1" {
  network_watcher_name      = azurerm_network_watcher.test.name
  resource_group_name       = azurerm_resource_group.test.name
  network_security_group_id = azurerm_network_security_group.test.id
  storage_account_id        = azurerm_storage_account.test.id
  enabled                   = true

  retention_policy {
    enabled = true
    days    = 89
  }
}

resource "azurerm_network_watcher_flow_log" "positive2" {
  network_watcher_name      = azurerm_network_watcher.test.name
  resource_group_name       = azurerm_resource_group.test.name
  network_security_group_id = azurerm_network_security_group.test.id
  storage_account_id        = azurerm_storage_account.test.id
  enabled                   = true

  retention_policy {
    enabled = true
    days    = 3
  }
}

resource "azurerm_network_watcher_flow_log" "positive3" {
  network_watcher_name      = azurerm_network_watcher.test.name
  resource_group_name       = azurerm_resource_group.test.name
  network_security_group_id = azurerm_network_security_group.test.id
  storage_account_id        = azurerm_storage_account.test.id
  enabled                   = true
}

resource "azurerm_network_watcher_flow_log" "positive4" {
  network_watcher_name      = azurerm_network_watcher.test.name
  resource_group_name       = azurerm_resource_group.test.name
  network_security_group_id = azurerm_network_security_group.test.id
  storage_account_id        = azurerm_storage_account.test.id
  enabled                   = true

  retention_policy {
    enabled = false
    days    = 900
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
    days    = 90
    }
}

resource "azurerm_network_watcher_flow_log" "negative2" {
    network_watcher_name = azurerm_network_watcher.test.name
    resource_group_name  = azurerm_resource_group.test.name
    network_security_group_id = azurerm_network_security_group.test.id
    storage_account_id        = azurerm_storage_account.test.id
    enabled                   = true

    retention_policy {
    enabled = true
    days    = 900
    }
}
```