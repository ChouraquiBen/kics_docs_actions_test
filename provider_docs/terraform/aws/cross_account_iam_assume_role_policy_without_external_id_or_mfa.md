---
title: "Cross-Account IAM Assume Role Policy Without ExternalId or MFA"
meta:
  name: "terraform/cross_account_iam_assume_role_policy_without_external_id_or_mfa"
  id: "09c35abf-5852-4622-ac7a-b987b331232e"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/cross_account_iam_assume_role_policy_without_external_id_or_mfa`
**Id:** `09c35abf-5852-4622-ac7a-b987b331232e`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Access Control
## Description
When creating cross-account IAM roles, it's crucial to implement additional security measures like External ID or Multi-Factor Authentication (MFA) to prevent unauthorized cross-account access. Without these safeguards, your resources become vulnerable to confused deputy attacks, where a malicious third party could trick your role into performing actions they shouldn't be authorized to do. To secure your configuration, add a Condition block to your assume role policy that requires either an External ID (as shown in the example below) or MFA validation:

```json
"Condition": {
  "StringEquals": {
    "sts:ExternalId": "98765"
  }
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role#assume_role_policy)

## Non-Compliant Code Examples
```aws
resource "aws_iam_role" "positive1" {
  name = "test_role"

  assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Principal": {
        "AWS": "arn:aws:iam::987654321145:root"
      },
      "Effect": "Allow",
      "Resource": "*",
      "Sid": ""
    }
  ]
}
EOF

  tags = {
    tag-key = "tag-value"
  }
}

```

```aws
resource "aws_iam_role" "positive3" {
  name = "test_role"

  assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": {
      "Action": "sts:AssumeRole",
      "Principal": {
        "AWS": "arn:aws:iam::987654321145:root"
      },
      "Effect": "Allow",
      "Resource": "*",
      "Sid": "",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": ""
        }
      }
  }
}
EOF

  tags = {
    tag-key = "tag-value"
  }
}

```

```aws
resource "aws_iam_role" "positive2" {
  name = "test_role"

  assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": {
      "Action": "sts:AssumeRole",
      "Principal": {
        "AWS": "arn:aws:iam::987654321145:root"
      },
      "Effect": "Allow",
      "Resource": "*",
      "Sid": "",
      "Condition": { 
         "Bool": { 
            "aws:MultiFactorAuthPresent": "false" 
          }
      }
  }
}
EOF

  tags = {
    tag-key = "tag-value"
  }
}

```

## Compliant Code Examples
```aws
resource "aws_iam_role" "negative1" {
  name = "test_role"

  assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Principal": {
        "AWS": "arn:aws:iam::987654321145:root"
      },
      "Effect": "Allow",
      "Resource": "*",
      "Sid": "",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": "98765"
        }
      }
    }
  ]
}
EOF

  tags = {
    tag-key = "tag-value"
  }
}

```

```aws
resource "aws_iam_role" "negative2" {
  name = "test_role"

  assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Principal": {
        "AWS": "arn:aws:iam::987654321145:root"
      },
      "Effect": "Allow",
      "Resource": "*",
      "Sid": "",
      "Condition": { 
         "Bool": { 
            "aws:MultiFactorAuthPresent": "true" 
          }
      }
    }
  ]
}
EOF

  tags = {
    tag-key = "tag-value"
  }
}

```