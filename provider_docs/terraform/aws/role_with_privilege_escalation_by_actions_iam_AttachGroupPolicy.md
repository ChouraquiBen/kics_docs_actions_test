---
title: "Role With Privilege Escalation By Actions 'iam:AttachGroupPolicy'"
meta:
  name: "terraform/role_with_privilege_escalation_by_actions_iam_AttachGroupPolicy"
  id: "f906113d-cdc0-415a-ba60-609cc6daaf4d"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/role_with_privilege_escalation_by_actions_iam_AttachGroupPolicy`
**Id:** `f906113d-cdc0-415a-ba60-609cc6daaf4d`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Granting the action `iam:AttachGroupPolicy` with the resource set to `*` in an AWS IAM role allows the entity to attach any group policy to any group in the AWS account, providing a path to privilege escalation. An attacker with this permission could leverage it to assign powerful permissions to groups they control or are a member of, thereby elevating their own privileges or those of other malicious accounts. If left unaddressed, this misconfiguration can result in unauthorized access or complete compromise of AWS resources, posing a serious security risk. It is critical to restrict the `iam:AttachGroupPolicy` action to specific, trusted resources and avoid using overly broad permissions in IAM policies.

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
          "iam:AttachGroupPolicy",
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