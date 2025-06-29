---
title: "Network Interfaces With Public IP"
meta:
  name: "terraform/network_interfaces_with_public_ip"
  id: "c1573577-e494-4417-8854-7e119368dc8b"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Networking and Firewall"
---
## Metadata
**Name:** `terraform/network_interfaces_with_public_ip`
**Id:** `c1573577-e494-4417-8854-7e119368dc8b`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Networking and Firewall
## Description
Exposing network interfaces with a public IP address in Terraform (`public_ip_address_id` attribute set) can introduce significant security risks, as it enables direct access from the internet to associated resources, increasing the attack surface for unauthorized access and attacks. If a public IP is required, additional controls and security baselines should be strictly enforced to minimize exposure. To enhance security, network interfaces should be defined without the `public_ip_address_id` field, as shown below:

```
resource "azurerm_network_interface" "secure" {
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

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_interface#public_ip_address_id)

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
    public_ip_address_id          = "IP"
  }
}

```

## Compliant Code Examples
```azure
resource "azurerm_network_interface" "negative" {
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