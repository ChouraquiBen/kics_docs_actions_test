---
title: "Group With Privilege Escalation By Actions 'iam:PutUserPolicy'"
meta:
  name: "terraform/group_with_privilege_escalation_by_actions_iam_PutUserPolicy"
  id: "60263b4a-6801-4587-911d-919c37ed733b"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/group_with_privilege_escalation_by_actions_iam_PutUserPolicy`
**Id:** `60263b4a-6801-4587-911d-919c37ed733b`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Allowing the action `iam:PutUserPolicy` on the `Resource` set to `"*"` in an inline group policy grants members of that group the ability to attach arbitrary permissions to any IAM user in the AWS account. This privilege escalation vulnerability could allow an attacker or compromised group member to grant themselves administrative access, bypass intended access controls, or compromise the entire AWS environment. To mitigate this risk, restrict the `iam:PutUserPolicy` action to specific users or resources and avoid policies with wildcard resources where possible, as shown below in a secure example:

```
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
          "iam:PutUserPolicy",
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