---
title: "Role With Privilege Escalation By Actions 'lambda:CreateFunction' And 'iam:PassRole' And 'lambda:InvokeFunction'"
meta:
  name: "terraform/role_with_privilege_escalation_by_actions_iam_PassRole_and_lambda_CreateFunction_lambda_InvokeFunction"
  id: "fa62ac4f-f5b9-45b9-97c1-625c8b6253ca"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/role_with_privilege_escalation_by_actions_iam_PassRole_and_lambda_CreateFunction_lambda_InvokeFunction`
**Id:** `fa62ac4f-f5b9-45b9-97c1-625c8b6253ca`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
This configuration grants an IAM role permission to create and invoke Lambda functions (`lambda:CreateFunction` and `lambda:InvokeFunction`) and to pass any IAM role (`iam:PassRole`) with the resource set to `"*"`. This combination of permissions enables privilege escalation, as a user with these rights can create a Lambda function that assumes any role in the account—including high-privilege roles such as `Administrator`. The attacker could then execute arbitrary actions with elevated privileges by passing critical roles to their malicious Lambda and invoking it. If left unaddressed, this vulnerability could allow unauthorized access to sensitive resources or full account takeover.

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
          "lambda:CreateFunction",
          "lambda:InvokeFunction"
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