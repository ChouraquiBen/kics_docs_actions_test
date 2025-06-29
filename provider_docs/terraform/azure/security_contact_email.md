---
title: "Security Contact Email"
meta:
  name: "terraform/security_contact_email"
  id: "34664094-59e0-4524-b69f-deaa1a68cce3"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Best Practices"
---
## Metadata
**Name:** `terraform/security_contact_email`
**Id:** `34664094-59e0-4524-b69f-deaa1a68cce3`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Best Practices
## Description
Defining a security contact email in the `azurerm_security_center_contact` resource is essential for ensuring that security alerts and notifications from Azure are sent to the correct personnel. If the `email` attribute is omitted, as shown below, important security incidents may go unnoticed, increasing the risk of delayed responses to threats:

```
resource "azurerm_security_center_contact" "insecure" {
  phone = "+1-555-555-5555"
  alert_notifications = true
  alerts_to_admins    = true
}
```

To address this, always specify the `email` attribute to ensure security alerts reach a monitored mailbox:

```
resource "azurerm_security_center_contact" "secure" {
  email = "contact@example.com"
  phone = "+1-555-555-5555"
  alert_notifications = true
  alerts_to_admins    = true
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/security_center_contact#email)

## Non-Compliant Code Examples
```azure
resource "azurerm_security_center_contact" "positive" {
  phone = "+1-555-555-5555"

  alert_notifications = true
  alerts_to_admins    = true
}

```

## Compliant Code Examples
```azure
resource "azurerm_security_center_contact" "negative" {
  email = "contact@example.com"
  phone = "+1-555-555-5555"

  alert_notifications = true
  alerts_to_admins    = true
}

```