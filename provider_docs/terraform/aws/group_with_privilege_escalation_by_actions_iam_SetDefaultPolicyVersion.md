---
title: "Group With Privilege Escalation By Actions 'iam:SetDefaultPolicyVersion'"
meta:
  name: "terraform/group_with_privilege_escalation_by_actions_iam_SetDefaultPolicyVersion"
  id: "7782d4b3-e23e-432b-9742-d9528432e771"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/group_with_privilege_escalation_by_actions_iam_SetDefaultPolicyVersion`
**Id:** `7782d4b3-e23e-432b-9742-d9528432e771`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
The configuration permits the `iam:SetDefaultPolicyVersion` action with a Resource value of `"*"`, meaning members of the `aws_iam_group.cosmic` group are allowed to set any version of any IAM policy as the default. This is a dangerous privilege escalation vector, as it could allow attackers to promote a malicious or overly permissive policy version, potentially granting themselves or others administrative access across AWS resources. If left unaddressed, this vulnerability could be exploited to bypass least privilege principles, resulting in unauthorized access or control over critical AWS infrastructure. To mitigate this risk, restrict the allowable actions and resources in IAM policies and avoid assigning wildcard `"*"` resource permissions to sensitive actions like `iam:SetDefaultPolicyVersion`.

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
          "iam:SetDefaultPolicyVersion",
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