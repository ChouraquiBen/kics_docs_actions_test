---
title: "IAM User With Access To Console"
meta:
  name: "terraform/iam_user_with_access_to_console"
  id: "9ec311bf-dfd9-421f-8498-0b063c8bc552"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/iam_user_with_access_to_console`
**Id:** `9ec311bf-dfd9-421f-8498-0b063c8bc552`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
This check ensures that AWS IAM users are not granted access to the AWS Management Console by omitting the creation of an associated login profile in Terraform configurations. Allowing console access exposes user credentials to potential phishing and brute-force attacks, especially if multifactor authentication is not enforced or passwords are weak. If left unaddressed, attackers could use compromised credentials to access AWS resources with the permissions of the affected user, potentially resulting in data leaks, resource manipulation, or unauthorized changes to cloud infrastructure. Removing console access for IAM users reduces the attack surface and encourages the use of more secure access methods, such as IAM roles and API keys with strict controls.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_user_login_profile)

## Non-Compliant Code Examples
```aws
resource "aws_iam_user" "example" {
  name          = "example"
  path          = "/"
  force_destroy = true
}

resource "aws_iam_user_login_profile" "example_login" {
  user    = aws_iam_user.example.name
  pgp_key = "keybase:some_person_that_exists"
}

```

## Compliant Code Examples
```aws
resource "aws_iam_user" "example" {
  name          = "example"
  path          = "/"
  force_destroy = true
}

```