---
title: "ECR Repository Not Encrypted With CMK"
meta:
  name: "terraform/ecr_repository_not_encrypted"
  id: "0e32d561-4b5a-4664-a6e3-a3fa85649157"
  cloud_provider: "terraform"
  severity: "LOW"
  category: "Encryption"
---
## Metadata
**Name:** `terraform/ecr_repository_not_encrypted`
**Id:** `0e32d561-4b5a-4664-a6e3-a3fa85649157`
**Cloud Provider:** terraform
**Severity:** Low
**Category:** Encryption
## Description
Amazon Elastic Container Registry (ECR) repositories should use customer-managed AWS KMS keys for encryption to ensure stronger access control, auditing, and compliance with organizational security requirements. By default, ECR repositories may only use AES256 encryption or omit the `encryption_configuration` block, which limits key management capabilities and centralized control over key lifecycle and access policies. A secure Terraform configuration example specifies a KMS key:

```
encryption_configuration {
  encryption_type = "KMS"
  kms_key = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab"
}
```

Without this, sensitive container images may be at greater risk of unauthorized access or inability to meet regulatory requirements for key rotation and audit.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ecr_repository#encryption_configuration)

## Non-Compliant Code Examples
```aws
resource "aws_ecr_repository" "foo" {
  name                 = "bar"
  image_tag_mutability = "IMMUTABLE"

  image_scanning_configuration {
    scan_on_push = true
  }
}

resource "aws_ecr_repository" "fooX" {
  name                 = "barX"
  image_tag_mutability = "IMMUTABLE"

  image_scanning_configuration {
    scan_on_push = true
  }

  encryption_configuration {
    encryption_type = "AES256"
  }
}

```

## Compliant Code Examples
```aws
resource "aws_ecr_repository" "foo2" {
  name                 = "bar"
  image_tag_mutability = "IMMUTABLE"

  image_scanning_configuration {
    scan_on_push = true
  }

  encryption_configuration {
    encryption_type = "KMS"
    kms_key = "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab"
  }
}

```