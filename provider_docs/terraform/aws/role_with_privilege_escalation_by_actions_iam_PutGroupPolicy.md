---
title: "Role With Privilege Escalation By Actions 'iam:PutGroupPolicy'"
meta:
  name: "terraform/role_with_privilege_escalation_by_actions_iam_PutGroupPolicy"
  id: "d6047119-a0b2-4b59-a4f2-127a36fb685b"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/role_with_privilege_escalation_by_actions_iam_PutGroupPolicy`
**Id:** `d6047119-a0b2-4b59-a4f2-127a36fb685b`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Granting the `iam:PutGroupPolicy` action with the `Resource` value set to `"*"` in an IAM policy, as shown below, allows the role to attach arbitrary permissions to any IAM group in the account:

```
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
```

This powerful permission can be abused for privilege escalation, as a malicious user assuming this role could grant themselves or others elevated privileges by attaching highly permissive group policies. To mitigate this risk, limit the scope of the `Resource` attribute to only the intended groups and restrict allowed actions to only those necessary for the role’s function.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy#policy)

## Non-Compliant Code Examples
```aws
resource "aws_iam_role" "cosmic" {
  name = "cosmic"
}

resource "aws_iam_role_policy" "test_inline_policy" {
  name = "test_inline_policy"
  role = aws_iam_role.cosmic.name

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