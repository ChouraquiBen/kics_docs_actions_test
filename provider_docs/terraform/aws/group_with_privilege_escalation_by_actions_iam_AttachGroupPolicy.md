---
title: "Group With Privilege Escalation By Actions 'iam:AttachGroupPolicy'"
meta:
  name: "terraform/group_with_privilege_escalation_by_actions_iam_AttachGroupPolicy"
  id: "70b42736-efee-4bce-80d5-50358ed94990"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/group_with_privilege_escalation_by_actions_iam_AttachGroupPolicy`
**Id:** `70b42736-efee-4bce-80d5-50358ed94990`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Allowing an IAM group the `iam:AttachGroupPolicy` action with a `Resource` set to `"*"` enables any user in that group to attach any policy, including those with administrator privileges, to any group. This creates a significant privilege escalation risk, where users can grant themselves or others far greater permissions than originally intended by attaching powerful policies. If left unaddressed, this can lead to full account compromise, unauthorized access, and loss of control over all AWS resources.

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