---
title: "Group With Privilege Escalation By Actions 'cloudformation:CreateStack' And 'iam:PassRole'"
meta:
  name: "terraform/group_with_privilege_escalation_by_actions_iam_PassRole_and_cloudformation_CreateStack"
  id: "9b0ffadc-a61f-4c2a-b1e6-68fab60f6267"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/group_with_privilege_escalation_by_actions_iam_PassRole_and_cloudformation_CreateStack`
**Id:** `9b0ffadc-a61f-4c2a-b1e6-68fab60f6267`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Allowing an IAM group broad permissions with `"cloudformation:CreateStack"` and `"iam:PassRole"` actions on all resources (`"Resource": "*"`) enables privilege escalation. With these permissions, a user can create a CloudFormation stack that provisions new IAM users, roles, or policies with elevated privileges and then use `iam:PassRole` to assume those roles, effectively bypassing intended access controls. If left unaddressed, attackers could gain unauthorized access to sensitive AWS resources or take full control of the account.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_group_policy#policy)

## Non-Compliant Code Examples
```aws
resource "aws_iam_group" "cosmic" {
  name = "cosmic"
}

resource "aws_iam_group_policy" "test_inline_policy" {
  name = "test_inline_policy"
  group = aws_iam_group.cosmic.name

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "cloudformation:CreateStack",
        ]
        Effect   = "Allow"
        Resource = "*"
      },
    ]
  })
}


resource "aws_iam_policy_attachment" "test-attach" {
  name       = "test-attachment"
  groups     = [aws_iam_group.cosmic.name]
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