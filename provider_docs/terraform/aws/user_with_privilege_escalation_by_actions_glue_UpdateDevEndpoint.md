---
title: "User With Privilege Escalation By Actions 'glue:UpdateDevEndpoint'"
meta:
  name: "terraform/user_with_privilege_escalation_by_actions_glue_UpdateDevEndpoint"
  id: "9b877bd8-94b4-4c10-a060-8e0436cc09fa"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/user_with_privilege_escalation_by_actions_glue_UpdateDevEndpoint`
**Id:** `9b877bd8-94b4-4c10-a060-8e0436cc09fa`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Allowing the `glue:UpdateDevEndpoint` action with the `Resource` attribute set to `"*"` in an AWS IAM policy enables broad and unrestricted management of AWS Glue development endpoints. This creates a serious privilege escalation vulnerability, as attackers with this permission can attach any IAM role to a Glue Dev Endpoint, potentially gaining access to additional permissions not intended for them. If left unaddressed, this misconfiguration may allow malicious users or compromised accounts to assume privileged roles and perform unauthorized actions across your AWS environment. It is critical to restrict sensitive actions like `glue:UpdateDevEndpoint` to only the required resources, and to avoid using wildcard ("*") resource definitions in IAM policies.

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
          "glue:UpdateDevEndpoint",
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