---
title: "Secret Expiration Not Set"
meta:
  name: "terraform/secret_expiration_not_set"
  id: "dfa20ffa-f476-428f-a490-424b41e91c7f"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Secret Management"
---
## Metadata
**Name:** `terraform/secret_expiration_not_set`
**Id:** `dfa20ffa-f476-428f-a490-424b41e91c7f`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Secret Management
## Description
Secrets stored in Azure Key Vault using the `azurerm_key_vault_secret` resource should always have an `expiration_date` set to ensure that sensitive credentials are not usable indefinitely. Failing to set an expiration date may result in forgotten or stale secrets lingering in your environment, increasing the risk of those secrets being misused if an account is compromised or a process changes. For a more secure configuration, explicitly specify the `expiration_date` attribute, as shown below:

```
resource "azurerm_key_vault_secret" "example" {
    name             = "api-key"
    value            = "supersecret"
    key_vault_id     = azurerm_key_vault.example.id

    expiration_date  = "2025-01-01T00:00:00Z"
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/key_vault_secret)

## Non-Compliant Code Examples
```azure
resource "azurerm_key_vault_secret" "positive1" {
    name         = "secret-sauce"
    value        = "szechuan"
    key_vault_id = azurerm_key_vault.example.id

    tags = {
    environment = "Production"
    }
}
```

## Compliant Code Examples
```azure
resource "azurerm_key_vault_secret" "negative1" {
    name         = "secret-sauce"
    value        = "szechuan"
    key_vault_id = azurerm_key_vault.example.id

    tags = {
    environment = "Production"
    }
    expiration_date = "2020-12-30T20:00:00Z"
}
```