---
title: "Certificate Has Expired"
meta:
  name: "terraform/certificate_has_expired"
  id: "c3831315-5ae6-4fa8-b458-3d4d5ab7a3f6"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/certificate_has_expired`
**Id:** `c3831315-5ae6-4fa8-b458-3d4d5ab7a3f6`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Expired SSL/TLS certificates should be removed from cloud resources to prevent the risk of exposing users to insecure or untrusted connections. When a resource, such as an AWS API Gateway custom domain, is configured with an expired certificate (e.g., `certificate_body = file("expiredCertificate.pem")`), clients attempting to access the API will receive security warnings, and automated clients may reject the connection entirely. This vulnerability undermines the integrity and trust of the service, potentially leading to denial of service, data interception, or man-in-the-middle attacks. Regularly updating and ensuring only valid certificates are used helps maintain secure encrypted communications between clients and services.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/api_gateway_rest_api)

## Non-Compliant Code Examples
```aws
resource "aws_api_gateway_domain_name" "example2" {
  certificate_body = file("expiredCertificate.pem")
  domain_name     = "api.example.com"
}


```

## Compliant Code Examples
```aws
resource "aws_api_gateway_domain_name" "example" {
  certificate_body = file("validCertificate.pem")
  domain_name     = "api.example.com"
}


```