---
title: "User With Privilege Escalation By Actions 'iam:AttachUserPolicy'"
meta:
  name: "terraform/user_with_privilege_escalation_by_actions_iam_AttachUserPolicy"
  id: "70cb518c-d990-46f6-bc05-44a5041493d6"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/user_with_privilege_escalation_by_actions_iam_AttachUserPolicy`
**Id:** `70cb518c-d990-46f6-bc05-44a5041493d6`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
This check identifies IAM policies that permit the action `iam:AttachUserPolicy` with the `Resource` field set to `"*"`. Granting this permission to a user, as shown below, allows them to attach any AWS managed or custom policy to any user, which can lead to privilege escalation and potential compromise of the AWS environment.

```
policy = jsonencode({
  Version = "2012-10-17"
  Statement = [
    {
      Action = [
        "iam:AttachUserPolicy",
      ]
      Effect   = "Allow"
      Resource = "*"
    },
  ]
})
```

If left unaddressed, an attacker with this permission could escalate their privileges far beyond what was originally intended, potentially gaining administrative access and leading to a full environment takeover.

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
          "iam:AttachUserPolicy",
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