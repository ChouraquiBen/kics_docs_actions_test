---
title: "User With Privilege Escalation By Actions 'iam:CreatePolicyVersion'"
meta:
  name: "terraform/user_with_privilege_escalation_by_actions_iam_CreatePolicyVersion"
  id: "1743f5f1-0bb0-4934-acef-c80baa5dadfa"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/user_with_privilege_escalation_by_actions_iam_CreatePolicyVersion`
**Id:** `1743f5f1-0bb0-4934-acef-c80baa5dadfa`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Allowing a user the `iam:CreatePolicyVersion` action with the resource set to `"*"` in Terraform, as shown in the policy statement below, enables them to update any IAM policy in the AWS account. This privilege can be exploited for privilege escalation, as the user could create a new policy version attaching elevated permissions to themselves or others.

```
policy = jsonencode({
  Version = "2012-10-17"
  Statement = [
    {
      Action = [
        "iam:CreatePolicyVersion",
      ]
      Effect   = "Allow"
      Resource = "*"
    },
  ]
})
```

If left unaddressed, this misconfiguration could allow an attacker to gain unauthorized administrative access or perform malicious actions across your AWS environment by changing permissions on any IAM policy.

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
          "iam:CreatePolicyVersion",
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