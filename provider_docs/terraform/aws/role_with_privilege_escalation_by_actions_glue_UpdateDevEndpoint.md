---
title: "Role With Privilege Escalation By Actions 'glue:UpdateDevEndpoint'"
meta:
  name: "terraform/role_with_privilege_escalation_by_actions_glue_UpdateDevEndpoint"
  id: "eda48c88-2b7d-4e34-b6ca-04c0194aee17"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/role_with_privilege_escalation_by_actions_glue_UpdateDevEndpoint`
**Id:** `eda48c88-2b7d-4e34-b6ca-04c0194aee17`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Granting the `glue:UpdateDevEndpoint` permission with the `Resource` set to `"*"` in an AWS IAM role introduces a privilege escalation risk. The `glue:UpdateDevEndpoint` action allows modification of existing AWS Glue DevEndpoints, including the ability to attach arbitrary IAM roles to these endpoints. An attacker with this permission could attach a role with higher privileges to a DevEndpoint and then use that role's credentials to perform unauthorized actions, bypassing intended security boundaries. If not addressed, this can lead to full account compromise or access to sensitive information by escalating the attacker's privileges within the AWS environment.

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
          "glue:UpdateDevEndpoint",
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