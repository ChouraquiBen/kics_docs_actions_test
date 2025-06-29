---
title: "CloudTrail Log Files Not Encrypted With KMS"
meta:
  name: "terraform/cloudtrail_log_files_not_encrypted_with_kms"
  id: "5d9e3164-9265-470c-9a10-57ae454ac0c7"
  cloud_provider: "terraform"
  severity: "LOW"
  category: "Encryption"
---
## Metadata
**Name:** `terraform/cloudtrail_log_files_not_encrypted_with_kms`
**Id:** `5d9e3164-9265-470c-9a10-57ae454ac0c7`
**Cloud Provider:** terraform
**Severity:** Low
**Category:** Encryption
## Description
CloudTrail logs contain sensitive information about account activity and should be protected from unauthorized access. If the `kms_key_id` attribute is not specified in the `aws_cloudtrail` resource block, as shown in:

```
resource "aws_cloudtrail" "positive1" {
  name           = "npositive_1"
  s3_bucket_name = "bucketlog_1"
}
```

then the logs stored in S3 are not encrypted with a customer-managed KMS key, leaving them vulnerable to exposure or tampering. Using the `kms_key_id` attribute, as in:

```
resource "aws_cloudtrail" "negative1" {
  name           = "negative1"
  s3_bucket_name = "bucketlog1"
  kms_key_id     = "arn:aws:kms:us-east-2:123456789012:key/12345678-1234-1234-1234-123456789012"
}
```

ensures that the logs are protected with strong encryption, reducing the risk of unauthorized access and helping meet compliance requirements.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudtrail#kms_key_id)

## Non-Compliant Code Examples
```aws
resource "aws_cloudtrail" "positive1" {
  name                          = "npositive_1"
  s3_bucket_name                = "bucketlog_1"
}

```

## Compliant Code Examples
```aws
resource "aws_cloudtrail" "negative1" {
  name                          = "negative1"
  s3_bucket_name                = "bucketlog1"
  kms_key_id                    = "arn:aws:kms:us-east-2:123456789012:key/12345678-1234-1234-1234-123456789012"
}

```