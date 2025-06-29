---
title: "Secretsmanager Secret Without KMS"
meta:
  name: "terraform/secretsmanager_secret_without_kms"
  id: "a2f548f2-188c-4fff-b172-e9a6acb216bd"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Encryption"
---
## Metadata
**Name:** `terraform/secretsmanager_secret_without_kms`
**Id:** `a2f548f2-188c-4fff-b172-e9a6acb216bd`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Encryption
## Description
By default, AWS Secrets Manager encrypts secrets using the default AWS managed key, which may not provide the desired level of control over key rotation, access policies, or auditability. Without explicitly specifying a `kms_key_id` in your Terraform resource, as shown below, secrets will not use a customer-managed AWS KMS key (CMK) for encryption:

```
resource "aws_secretsmanager_secret" "example" {
  name = "example"
}
```

This misconfiguration can increase the exposure of sensitive data and limit your ability to implement strict access controls. To reduce risk, explicitly provide a `kms_key_id` attribute referencing a CMK:

```
resource "aws_secretsmanager_secret" "example" {
  name       = "example"
  kms_key_id = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab"
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret#kms_key_id)

## Non-Compliant Code Examples
```aws
resource "aws_secretsmanager_secret" "example" {
  name = "example"
}

```

## Compliant Code Examples
```aws
resource "aws_secretsmanager_secret" "example" {
  name = "example"
  kms_key_id = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab"
}

```