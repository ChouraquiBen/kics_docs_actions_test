---
title: "SNS Topic Encrypted With AWS Managed Key"
meta:
  name: "terraform/sns_topic_encrypted_with_aws_managed_key"
  id: "b1a72f66-2236-4f3b-87ba-0da1b366956f"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Encryption"
---
## Metadata
**Name:** `terraform/sns_topic_encrypted_with_aws_managed_key`
**Id:** `b1a72f66-2236-4f3b-87ba-0da1b366956f`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Encryption
## Description
SNS (Simple Notification Service) topics should be encrypted using customer-managed AWS KMS keys, rather than default AWS-managed keys, to provide greater control over access and auditing. Using an AWS-managed key (such as `alias/aws/sns`) limits visibility into key usage and does not allow setting granular key rotation or access policies tailored to an organization's specific requirements. If left unaddressed, messages published to the SNS topic are protected only by the generic AWS-managed key, increasing the risk that sensitive information could be accessed by unauthorized users or compromise key compliance obligations. This misconfiguration could lead to operational and regulatory risks if message confidentiality is critical.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic#kms_master_key_id)

## Non-Compliant Code Examples
```aws
resource "aws_sns_topic" "user_updates" {
  name              = "user-updates-topic"
  kms_master_key_id = "alias/aws/sns"
}

```

```aws
provider "aws" {
  region = "us-east-1"
}

data "aws_kms_key" "by_alias" {
  key_id = "alias/aws/sns"
}

resource "aws_sns_topic" "test" {
  name              = "sns_ecnrypted"
  kms_master_key_id = data.aws_kms_key.by_alias.arn
}

```

## Compliant Code Examples
```aws
provider "aws2" {
  region = "us-east-1"
}

resource "aws_sns_topic" "test2" {
  name              = "sns_ecnrypted"
  kms_master_key_id = "alias/MyAlias"
}

```