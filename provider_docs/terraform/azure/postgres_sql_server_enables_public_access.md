---
title: "Ensure that PostgreSQL server disables public network access"
meta:
  name: "terraform/postgres_sql_server_enables_public_access"
  id: "c7f8e9a1-b2c3-d4e5-f6a7-8901b2c3d4e5"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Networking and Firewall"
---
## Metadata
**Name:** `terraform/postgres_sql_server_enables_public_access`
**Id:** `c7f8e9a1-b2c3-d4e5-f6a7-8901b2c3d4e5`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Networking and Firewall
## Description
PostgreSQL servers with public network access enabled are directly accessible from the internet, potentially exposing sensitive data to unauthorized users and increasing the attack surface. When public_network_access_enabled is set to true or omitted (which defaults to true), your database becomes vulnerable to brute force attacks, SQL injection attempts, and other exploits originating from the public internet. To mitigate this risk, always explicitly set public_network_access_enabled to false as shown in the following secure example: `public_network_access_enabled = [false]`. For additional security, consider implementing private endpoints, firewalls, and virtual network service endpoints.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/postgresql_server)

## Non-Compliant Code Examples
```azure
# Failing example 1: Attribute exists but is set to true.
resource "azurerm_postgresql_server" "bad_example" {
  name                = "bad-postgresql-server"
  location            = "East US"
  resource_group_name = "example-rg"

  public_network_access_enabled = [true] # ❌ Should be false

  version                 = "9.6"
  ssl_enforcement_enabled = true
  sku_name                = "B_Gen5_1"
}

# Failing example 2: Attribute is missing.
resource "azurerm_postgresql_server" "bad_example_missing" {
  name                = "bad-postgresql-server-missing"
  location            = "East US"
  resource_group_name = "example-rg"
  # public_network_access_enabled is not defined → ❌ Violation

  version                 = "9.6"
  ssl_enforcement_enabled = true
  sku_name                = "B_Gen5_1"
}

```

## Compliant Code Examples
```azure
resource "azurerm_postgresql_server" "good_example" {
  name                = "good-postgresql-server"
  location            = "East US"
  resource_group_name = "example-rg"

  public_network_access_enabled = [false] # ✅ Correct setting

  version                 = "9.6"
  ssl_enforcement_enabled = true
  sku_name                = "B_Gen5_1"
}

```