---
title: "Role With Privilege Escalation By Actions 'cloudformation:CreateStack' And 'iam:PassRole'"
meta:
  name: "terraform/role_with_privilege_escalation_by_actions_iam_PassRole_and_cloudformation_CreateStack"
  id: "be2aa235-bd93-4b68-978a-1cc65d49082f"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/role_with_privilege_escalation_by_actions_iam_PassRole_and_cloudformation_CreateStack`
**Id:** `be2aa235-bd93-4b68-978a-1cc65d49082f`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Granting an IAM role permissions for both `cloudformation:CreateStack` and `iam:PassRole` actions with the resource set to `"*"` allows users with this role to launch CloudFormation stacks that assume any IAM role in the account, leading to privilege escalation. This vulnerability means an attacker could potentially create resources with elevated privileges or gain full administrative access to the AWS environment. To mitigate this, avoid assigning overly permissive policies; restrict `iam:PassRole` and `cloudformation:CreateStack` to only trusted roles and explicitly specify the allowed resource ARNs in the policy's `Resource` attribute.

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
  roles      = [aws_iam_role.cosmic.name]
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