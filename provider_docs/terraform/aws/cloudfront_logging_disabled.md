---
title: "CloudFront Logging Disabled"
meta:
  name: "terraform/cloudfront_logging_disabled"
  id: "94690d79-b3b0-43de-b656-84ebef5753e5"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Observability"
---
## Metadata
**Name:** `terraform/cloudfront_logging_disabled`
**Id:** `94690d79-b3b0-43de-b656-84ebef5753e5`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Observability
## Description
Enabling logging for AWS CloudFront distributions is critical to ensure that all viewer requests are captured for security monitoring, troubleshooting, and compliance. Logging is enabled in Terraform by including the `logging_config` attribute within the `aws_cloudfront_distribution` resource, as shown below:

```
resource "aws_cloudfront_distribution" "logging_enabled" {
  // ... other configuration ...
  logging_config {
    include_cookies = false
    bucket          = "mylogs.s3.amazonaws.com"
    prefix          = "myprefix"
  }
}
```

Without logging enabled, malicious or unauthorized access patterns can go undetected, leaving the distribution vulnerable to abuse and limiting forensic analysis capabilities in the event of an incident.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudfront_distribution)

## Non-Compliant Code Examples
```aws
resource "aws_cloudfront_distribution" "positive1" {
  origin {
    domain_name = aws_s3_bucket.b.bucket_regional_domain_name
    origin_id   = local.s3_origin_id

    s3_origin_config {
      origin_access_identity = "origin-access-identity/cloudfront/ABCDEFG1234567"
    }
  }

  enabled             = true
  is_ipv6_enabled     = true
  comment             = "Some comment"
  default_root_object = "index.html"

  }
  
```

## Compliant Code Examples
```aws
resource "aws_cloudfront_distribution" "negative1" {
  origin {
    domain_name = aws_s3_bucket.b.bucket_regional_domain_name
    origin_id   = local.s3_origin_id

    s3_origin_config {
      origin_access_identity = "origin-access-identity/cloudfront/ABCDEFG1234567"
    }
  }

  enabled             = true
  is_ipv6_enabled     = true
  comment             = "Some comment"
  default_root_object = "index.html"

  logging_config {
    include_cookies = false
    bucket          = "mylogs.s3.amazonaws.com"
    prefix          = "myprefix"
  }
  
}
```