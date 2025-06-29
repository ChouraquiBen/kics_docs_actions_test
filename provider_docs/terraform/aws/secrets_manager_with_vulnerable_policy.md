---
title: "Secrets Manager With Vulnerable Policy"
meta:
  name: "terraform/secrets_manager_with_vulnerable_policy"
  id: "fa00ce45-386d-4718-8392-fb485e1f3c5b"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/secrets_manager_with_vulnerable_policy`
**Id:** `fa00ce45-386d-4718-8392-fb485e1f3c5b`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Access Control
## Description
AWS Secrets Manager policies with wildcards in 'Principal' or 'Action' fields create significant security risks by potentially granting excessive permissions to unintended entities. When '*' is used in the Principal field, any AWS identity can access your secrets, and when used in the Action field, it allows all possible operations on those secrets. This overly permissive access violates the principle of least privilege and could lead to unauthorized access or manipulation of sensitive information. Instead of using wildcards, specify exact identities and permissions as shown in the secure example: `"Principal": {"AWS": "arn:aws:iam::var.account_id:saml-provider/var.provider_name"}` and `"Action": "secretsmanager:GetSecretValue"`.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret_policy#policy)

## Non-Compliant Code Examples
```aws
provider "aws" {
  region = "us-east-1"
}

resource "aws_secretsmanager_secret" "not_secure_policy" {
  name = "not_secure_secret"
}

resource "aws_secretsmanager_secret_policy" "example" {
  secret_arn = aws_secretsmanager_secret.not_secure_policy.arn

  policy = <<POLICY
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "EnableAllPermissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "secretsmanager:*",
      "Resource": "*"
    }
  ]
}
POLICY
}

```

## Compliant Code Examples
```aws
resource "aws_secretsmanager_secret" "example2" {
  name = "example"
}

resource "aws_secretsmanager_secret_policy" "example2" {
  secret_arn = aws_secretsmanager_secret.example2.arn

  policy = <<POLICY
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "EnableAllPermissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::var.account_id:saml-provider/var.provider_name"
      },
      "Action": "secretsmanager:GetSecretValue",
      "Resource": "*"
    }
  ]
}
POLICY
}

```