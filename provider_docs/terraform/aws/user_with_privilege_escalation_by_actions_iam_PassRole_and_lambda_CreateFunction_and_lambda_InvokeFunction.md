---
title: "User With Privilege Escalation By Actions 'lambda:CreateFunction' And 'iam:PassRole' And 'lambda:InvokeFunction'"
meta:
  name: "terraform/user_with_privilege_escalation_by_actions_iam_PassRole_and_lambda_CreateFunction_and_lambda_InvokeFunction"
  id: "8055dec2-efb8-4fe6-8837-d9bed6ff202a"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/user_with_privilege_escalation_by_actions_iam_PassRole_and_lambda_CreateFunction_and_lambda_InvokeFunction`
**Id:** `8055dec2-efb8-4fe6-8837-d9bed6ff202a`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Granting a user the permissions `'lambda:CreateFunction'`, `'lambda:InvokeFunction'`, and `'iam:PassRole'` with the `Resource` set to `"*"` allows them to create and execute Lambda functions under any IAM role, potentially escalating their privileges in the AWS environment. This misconfiguration means the user can attach highly privileged roles to their Lambda functions and run them, effectively gaining any permissions those roles have—including full administrative access—without approval or oversight. If left unaddressed, this could lead to complete compromise of AWS resources, data theft, or account takeover.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_user_policy#policy)

## Non-Compliant Code Examples
```aws
resource "aws_iam_user" "cosmic" {
  name = "cosmic"
}

resource "aws_iam_user_policy" "test_inline_policy" {
  name = "test_inline_policy"
  user = aws_iam_user.cosmic.name

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "lambda:CreateFunction",
          "lambda:InvokeFunction"
        ]
        Effect   = "Allow"
        Resource = "*"
      },
    ]
  })
}


resource "aws_iam_policy_attachment" "test-attach" {
  name       = "test-attachment"
  users      = [aws_iam_user.cosmic.name]
  roles      = [aws_iam_role.role.name]
  groups     = [aws_iam_group.group.name]
  policy_arn = aws_iam_policy.policy.arn
}


resource "aws_iam_policy" "policy" {
  name        = "test-policy"
  description = "A test policy"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "iam:PassRole",
        ]
        Effect   = "Allow"
        Resource = "*"
      },
    ]
  })
}

```

## Compliant Code Examples
```aws
resource "aws_iam_user" "cosmic2" {
  name = "cosmic2"
}

resource "aws_iam_user_policy" "inline_policy_run_instances2" {
  name = "inline_policy_run_instances"
  user = aws_iam_user.cosmic2.name

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "ec2:Describe*",
        ]
        Effect   = "Allow"
        Resource = "*"
      },
    ]
  })
}

```