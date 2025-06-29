---
title: "CMK Rotation Disabled"
meta:
  name: "terraform/cmk_rotation_disabled"
  id: "22fbfeac-7b5a-421a-8a27-7a2178bb910b"
  cloud_provider: "terraform"
  severity: "LOW"
  category: "Observability"
---
## Metadata
**Name:** `terraform/cmk_rotation_disabled`
**Id:** `22fbfeac-7b5a-421a-8a27-7a2178bb910b`
**Cloud Provider:** terraform
**Severity:** Low
**Category:** Observability
## Description
Customer Master Keys (CMKs) created using AWS Key Management Service (KMS) should have automatic key rotation enabled to enhance cryptographic security. Failing to set the `enable_key_rotation` attribute to `true` may increase the risk of key compromise, as cryptographic keys used over extended periods are more susceptible to brute-force or other attacks. Enabling rotation ensures the key material is automatically replaced annually, reducing exposure and supporting compliance with security best practices.

```
resource "aws_kms_key" "example" {
  description         = "KMS key with rotation enabled"
  is_enabled          = true
  enable_key_rotation = true
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kms_key#enable_key_rotation)

## Non-Compliant Code Examples
```aws
resource "aws_kms_key" "positive1" {
  description = "KMS key 1"
}

```

```aws
resource "aws_kms_key" "positive4" {
  description              = "KMS key 4"
  customer_master_key_spec = "SYMMETRIC_DEFAULT"
  enable_key_rotation      = false
}

```

```aws
resource "aws_kms_key" "positive3" {
  description              = "KMS key 3"
  is_enabled               = true
  customer_master_key_spec = "SYMMETRIC_DEFAULT"
  enable_key_rotation      = false
}

```

## Compliant Code Examples
```aws
resource "aws_kms_key" "negative1" {
  description         = "KMS key 1"
  is_enabled          = true
  enable_key_rotation = true
}

```

```aws
resource "aws_kms_key" "negative3" {
  description              = "KMS key 3"
  customer_master_key_spec = "RSA_2048"
}

```

```aws
resource "aws_kms_key" "negative2" {
  description              = "KMS key 2"
  customer_master_key_spec = "RSA_4096"
}

```