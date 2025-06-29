---
title: "Azure Instance Using Basic Authentication"
meta:
  name: "terraform/azure_instance_using_basic_authentication"
  id: "dafe30ec-325d-4516-85d1-e8e6776f012c"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Best Practices"
---
## Metadata
**Name:** `terraform/azure_instance_using_basic_authentication`
**Id:** `dafe30ec-325d-4516-85d1-e8e6776f012c`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Best Practices
## Description
Allowing basic password authentication for Azure virtual machines introduces a significant security risk, as passwords can be easily guessed, brute-forced, or leaked. When `disable_password_authentication = false` is set in the `os_profile_linux_config` block, as shown below, the VM permits password-based SSH logins:

```
os_profile_linux_config {
  disable_password_authentication = false
}
```

Instead, SSH key authentication should be required to ensure secure, cryptographically strong access to the instance, as in the following configuration:

```
admin_ssh_key {
  username   = "adminuser"
  public_key = file("~/.ssh/id_rsa.pub")
}
```

Failure to enforce SSH key authentication can lead to unauthorized access, data breaches, or compromise of critical resources.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/linux_virtual_machine#admin_ssh_key)

## Non-Compliant Code Examples
```azure
resource "azurerm_virtual_machine" "positive1" {
  name                  = "${var.prefix}-vm"
  location              = azurerm_resource_group.main.location
  resource_group_name   = azurerm_resource_group.main.name
  network_interface_ids = []
  vm_size               = "Standard_DS1_v2"

  os_profile_linux_config {
    disable_password_authentication = false
  }
}

```

```azure
resource "azurerm_linux_virtual_machine" "positive1" {
  name                  = "${var.prefix}-vm"
  location              = azurerm_resource_group.main.location
  resource_group_name   = azurerm_resource_group.main.name
  network_interface_ids = []
  vm_size               = "Standard_DS1_v2"
  disable_password_authentication = false
}

```

## Compliant Code Examples
```azure
resource "azurerm_linux_virtual_machine" "negative1" {
  name                  = "${var.prefix}-vm"
  location              = azurerm_resource_group.main.location
  resource_group_name   = azurerm_resource_group.main.name
  network_interface_ids = [azurerm_network_interface.main.id]
  vm_size               = "Standard_DS1_v2"

  admin_ssh_key {
    username   = "adminuser"
    public_key = file("~/.ssh/id_rsa.pub")
  }
}

```

```azure
resource "azurerm_virtual_machine" "negative1" {
  name                  = "${var.prefix}-vm"
  location              = azurerm_resource_group.main.location
  resource_group_name   = azurerm_resource_group.main.name
  network_interface_ids = [azurerm_network_interface.main.id]
  vm_size               = "Standard_DS1_v2"

  os_profile_linux_config {
    disable_password_authentication = true
  }

  admin_ssh_key {
    username   = "adminuser"
    public_key = file("~/.ssh/id_rsa.pub")
  }
}

```