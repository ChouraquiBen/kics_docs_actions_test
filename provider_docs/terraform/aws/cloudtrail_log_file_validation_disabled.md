---
title: "CloudTrail Log File Validation Disabled"
meta:
  name: "terraform/cloudtrail_log_file_validation_disabled"
  id: "52ffcfa6-6c70-4ea6-8376-d828d3961669"
  cloud_provider: "terraform"
  severity: "LOW"
  category: "Observability"
---
## Metadata
**Name:** `terraform/cloudtrail_log_file_validation_disabled`
**Id:** `52ffcfa6-6c70-4ea6-8376-d828d3961669`
**Cloud Provider:** terraform
**Severity:** Low
**Category:** Observability
## Description
CloudTrail log file validation should be enabled by setting the `enable_log_file_validation` attribute to `true` in the `aws_cloudtrail` resource. This ensures that CloudTrail computes and stores a hash for every log file it delivers, allowing detection of any tampering or unauthorized modifications of log files. If log file validation is not enabled, malicious actors could alter or delete logs without detection, undermining the integrity of audit trails and hampering forensic investigations.

```
resource "aws_cloudtrail" "example" {
  name                       = "example"
  s3_bucket_name             = "bucketlog"
  enable_log_file_validation = true
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudtrail#enable_log_file_validation)

## Non-Compliant Code Examples
```aws
resource "aws_cloudtrail" "positive1" {
  name                          = "positive1"
  s3_bucket_name                = "bucketlog1"
}

resource "aws_cloudtrail" "positive2" {
  name                          = "positive2"
  s3_bucket_name                = "bucketlog2"
  enable_log_file_validation    = false
}

```

## Compliant Code Examples
```aws
resource "aws_cloudtrail" "negative1" {
  name                          = "negative1"
  s3_bucket_name                = "bucketlog1"
  enable_log_file_validation    = true
}

```