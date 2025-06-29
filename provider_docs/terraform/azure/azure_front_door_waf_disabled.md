---
title: "Azure Front Door WAF Disabled"
meta:
  name: "terraform/azure_front_door_waf_disabled"
  id: "835a4f2f-df43-437d-9943-545ccfc55961"
  cloud_provider: "terraform"
  severity: "LOW"
  category: "Networking and Firewall"
---
## Metadata
**Name:** `terraform/azure_front_door_waf_disabled`
**Id:** `835a4f2f-df43-437d-9943-545ccfc55961`
**Cloud Provider:** terraform
**Severity:** Low
**Category:** Networking and Firewall
## Description
Azure Front Door should have a Web Application Firewall (WAF) enabled to protect applications from common web vulnerabilities and attacks such as SQL injection and cross-site scripting. If the `web_application_firewall_policy_link_id` attribute is not configured for the `frontend_endpoint` block, malicious traffic can reach backend resources without any inspection or filtering, increasing the risk of exploitation. To address this, ensure that WAF is linked as shown below:

```
frontend_endpoint {
  name      = "exampleFrontendEndpoint1"
  host_name = "example-FrontDoor.azurefd.net"
  web_application_firewall_policy_link_id = "id"
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/frontdoor#web_application_firewall_policy_link_id)

## Non-Compliant Code Examples
```azure
resource "azurerm_frontdoor" "positive" {
  name                                         = "example-FrontDoor"
  resource_group_name                          = azurerm_resource_group.example.name
  enforce_backend_pools_certificate_name_check = false

  routing_rule {
    name               = "exampleRoutingRule1"
    accepted_protocols = ["Http", "Https"]
    patterns_to_match  = ["/*"]
    frontend_endpoints = ["exampleFrontendEndpoint1"]
    forwarding_configuration {
      forwarding_protocol = "MatchRequest"
      backend_pool_name   = "exampleBackendBing"
    }
  }

  backend_pool_load_balancing {
    name = "exampleLoadBalancingSettings1"
  }

  backend_pool_health_probe {
    name = "exampleHealthProbeSetting1"
  }

  backend_pool {
    name = "exampleBackendBing"
    backend {
      host_header = "www.bing.com"
      address     = "www.bing.com"
      http_port   = 80
      https_port  = 443
    }

    load_balancing_name = "exampleLoadBalancingSettings1"
    health_probe_name   = "exampleHealthProbeSetting1"
  }

  frontend_endpoint {
    name      = "exampleFrontendEndpoint1"
    host_name = "example-FrontDoor.azurefd.net"
  }
}

```

## Compliant Code Examples
```azure
resource "azurerm_frontdoor" "negative" {
  name                                         = "example-FrontDoor"
  resource_group_name                          = azurerm_resource_group.example.name
  enforce_backend_pools_certificate_name_check = false

  routing_rule {
    name               = "exampleRoutingRule1"
    accepted_protocols = ["Http", "Https"]
    patterns_to_match  = ["/*"]
    frontend_endpoints = ["exampleFrontendEndpoint1"]
    forwarding_configuration {
      forwarding_protocol = "MatchRequest"
      backend_pool_name   = "exampleBackendBing"
    }
  }

  backend_pool_load_balancing {
    name = "exampleLoadBalancingSettings1"
  }

  backend_pool_health_probe {
    name = "exampleHealthProbeSetting1"
  }

  backend_pool {
    name = "exampleBackendBing"
    backend {
      host_header = "www.bing.com"
      address     = "www.bing.com"
      http_port   = 80
      https_port  = 443
    }

    load_balancing_name = "exampleLoadBalancingSettings1"
    health_probe_name   = "exampleHealthProbeSetting1"
  }

  frontend_endpoint {
    name      = "exampleFrontendEndpoint1"
    host_name = "example-FrontDoor.azurefd.net"
    web_application_firewall_policy_link_id = "id"
  }
}

```