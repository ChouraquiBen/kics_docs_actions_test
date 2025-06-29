---
title: "User With Privilege Escalation By Actions 'iam:PutGroupPolicy'"
meta:
  name: "terraform/user_with_privilege_escalation_by_actions_iam_PutGroupPolicy"
  id: "8bfbf7ab-d5e8-4100-8618-798956e101e0"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/user_with_privilege_escalation_by_actions_iam_PutGroupPolicy`
**Id:** `8bfbf7ab-d5e8-4100-8618-798956e101e0`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Granting an IAM user the `iam:PutGroupPolicy` action with `"Resource": "*"` allows them to attach arbitrary inline policies to any IAM group in the AWS account, leading to potential privilege escalation. With this permission, a user could assign powerful or administrative permissions to groups they belong to, effectively bypassing intended security boundaries. To prevent this, it's important to restrict IAM user actions to the minimum necessary and avoid using wildcards in sensitive permissions, as shown below:

```
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
```

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
          "iam:PutGroupPolicy",
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