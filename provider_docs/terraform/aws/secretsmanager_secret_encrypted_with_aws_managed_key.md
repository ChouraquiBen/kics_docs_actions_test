---
title: "Secretsmanager Secret Encrypted With AWS Managed Key"
meta:
  name: "terraform/secretsmanager_secret_encrypted_with_aws_managed_key"
  id: "b0d3ef3f-845d-4b1b-83d6-63a5a380375f"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Encryption"
---
## Metadata
**Name:** `terraform/secretsmanager_secret_encrypted_with_aws_managed_key`
**Id:** `b0d3ef3f-845d-4b1b-83d6-63a5a380375f`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Encryption
## Description
AWS Secrets Manager secrets should be encrypted with customer-managed KMS keys rather than the default AWS managed keys. Relying on AWS managed keys limits an organization's ability to control, rotate, and audit encryption keys, which are important factors in enforcing robust security policies and compliance requirements. Without customer-managed KMS keys, there may be a greater risk of unauthorized access or insufficient key lifecycle management. If left unaddressed, sensitive information stored in Secrets Manager could be compromised due to weaker or less transparent key management practices.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret#kms_key_id)

## Non-Compliant Code Examples
```aws
resource "aws_secretsmanager_secret" "test2" {
  name       = "test-cloudrail-1"
  kms_key_id = "alias/aws/secretsmanager"
}

```

```aws
provider "aws" {
  region = "us-east-1"
}

data "aws_kms_key" "by_alias" {
  key_id = "alias/aws/secretsmanager"
}

resource "aws_secretsmanager_secret" "test" {
  name       = "test-cloudrail-1"
  kms_key_id = data.aws_kms_key.by_alias.arn
}

```

## Compliant Code Examples
```aws
resource "aws_secretsmanager_secret" "test222" {
  name       = "test-cloudrail-1"
  kms_key_id = "alias/MyAlias"
}


```