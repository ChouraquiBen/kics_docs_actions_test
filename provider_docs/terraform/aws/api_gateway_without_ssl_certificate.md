---
title: "API Gateway Without SSL Certificate"
meta:
  name: "terraform/api_gateway_without_ssl_certificate"
  id: "0b4869fc-a842-4597-aa00-1294df425440"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/api_gateway_without_ssl_certificate`
**Id:** `0b4869fc-a842-4597-aa00-1294df425440`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Configurations
## Description
When configuring an `aws_api_gateway_stage` resource in Terraform, the `client_certificate_id` attribute should be set to enable SSL client certificate authentication. Without specifying `client_certificate_id`, clients can access your API Gateway stage without presenting a valid client-side certificate, leaving the API vulnerable to unauthorized access. Enabling this attribute, as shown below, ensures that only clients with a valid certificate can establish SSL connections:

```
resource "aws_api_gateway_stage" "example" {
  stage_name            = "prod"
  rest_api_id           = aws_api_gateway_rest_api.test.id
  deployment_id         = aws_api_gateway_deployment.test.id
  client_certificate_id = "12131323"
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/api_gateway_stage#client_certificate_id)

## Non-Compliant Code Examples
```aws
resource "aws_api_gateway_stage" "positive1" {
  stage_name    = "prod"
  rest_api_id   = aws_api_gateway_rest_api.test.id
  deployment_id = aws_api_gateway_deployment.test.id

}

```

## Compliant Code Examples
```aws
resource "aws_api_gateway_stage" "negative1" {
  stage_name    = "prod"
  rest_api_id   = aws_api_gateway_rest_api.test.id
  deployment_id = aws_api_gateway_deployment.test.id


  client_certificate_id = "12131323"

}

```