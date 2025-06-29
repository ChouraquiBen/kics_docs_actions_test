---
title: "IAM Password Policy Does Not Require Uppercase Letter"
meta:
  name: "terraform/iam_password_does_not_require_uppercase"
  id: "2c3d4ghwt-e5f6-7890-ab12-cd34ef567890"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Best Practices"
---
## Metadata
**Name:** `terraform/iam_password_does_not_require_uppercase`
**Id:** `2c3d4ghwt-e5f6-7890-ab12-cd34ef567890`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Best Practices
## Description
This check ensures that the AWS IAM password policy requires users to include at least one uppercase letter in their passwords. Without enforcing uppercase characters, passwords become more susceptible to brute-force or dictionary attacks, as the possible character space is significantly reduced. This weakens account security and increases the risk of unauthorized access to critical resources. Enforcing a strong password policy, including uppercase letter requirements, helps protect sensitive AWS environments from compromise due to easily guessed or weak passwords.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_account_password_policy#require_uppercase_characters)

## Non-Compliant Code Examples
```aws
resource "aws_iam_account_password_policy" "bad_example" {
  minimum_password_length      = 14
  require_symbols              = true
  require_numbers              = true
  require_lowercase_characters = true
  require_uppercase_characters = false
}
```

## Compliant Code Examples
```aws
resource "aws_iam_account_password_policy" "good_example" {
  minimum_password_length      = 14
  require_symbols              = true
  require_numbers              = true
  require_lowercase_characters = true
  require_uppercase_characters = true
}

```