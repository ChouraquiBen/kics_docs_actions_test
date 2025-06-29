---
title: "Network Interfaces IP Forwarding Enabled"
meta:
  name: "terraform/network_interfaces_ip_forwarding_enabled"
  id: "4216ebac-d74c-4423-b437-35025cb88af5"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Networking and Firewall"
---
## Metadata
**Name:** `terraform/network_interfaces_ip_forwarding_enabled`
**Id:** `4216ebac-d74c-4423-b437-35025cb88af5`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Networking and Firewall
## Description
Enabling IP forwarding on network interfaces allows packets to be routed between networks, which can make the network interface behave like a router. This may expose your environment to lateral movement and man-in-the-middle attacks if an attacker gains access to the interface. To prevent this risk, set the `enable_ip_forwarding` attribute to `false` in your Terraform configuration, as shown below:

```
resource "azurerm_network_interface" "secure" {
  // ... other configuration ...
  enable_ip_forwarding = false
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_interface#enable_ip_forwarding)

## Non-Compliant Code Examples
```azure
resource "azurerm_network_interface" "positive" {
  name                = "example-nic"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.example.id
    private_ip_address_allocation = "Dynamic"
  }

  enable_ip_forwarding = true
}

```

## Compliant Code Examples
```azure
resource "azurerm_network_interface" "negative1" {
  name                = "example-nic"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.example.id
    private_ip_address_allocation = "Dynamic"
  }

  enable_ip_forwarding = false
}

```

```azure
resource "azurerm_network_interface" "negative2" {
  name                = "example-nic"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.example.id
    private_ip_address_allocation = "Dynamic"
  }
}

```