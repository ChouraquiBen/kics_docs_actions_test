---
title: "User With Privilege Escalation By Actions 'iam:CreateAccessKey'"
meta:
  name: "terraform/user_with_privilege_escalation_by_actions_iam_CreateAccessKey"
  id: "113208f2-a886-4526-9ecc-f3218600e12c"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/user_with_privilege_escalation_by_actions_iam_CreateAccessKey`
**Id:** `113208f2-a886-4526-9ecc-f3218600e12c`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Granting a user the `iam:CreateAccessKey` action with a resource set to `"*"` allows that user to create access keys for any IAM user in the AWS account. This over-privileged configuration, as shown below, introduces a privilege escalation risk, as the user could generate access keys for higher-privileged users and gain unauthorized access to sensitive resources:

```
policy = jsonencode({
  Version = "2012-10-17"
  Statement = [
    {
      Action = [
        "iam:CreateAccessKey",
      ]
      Effect   = "Allow"
      Resource = "*"
    },
  ]
})
```

To mitigate this risk, limit the action and resource to the specific user needing access, or scope the permissions more narrowly to avoid unauthorized key creation.

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
          "iam:CreateAccessKey",
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