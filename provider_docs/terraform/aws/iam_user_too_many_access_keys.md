---
title: "IAM User Has Too Many Access Keys"
meta:
  name: "terraform/iam_user_too_many_access_keys"
  id: "3561130e-9c5f-485b-9e16-2764c82763e5"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/iam_user_too_many_access_keys`
**Id:** `3561130e-9c5f-485b-9e16-2764c82763e5`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Configurations
## Description
IAM users should not have more than one active access key at a time, as shown by multiple `aws_iam_access_key` resources provisioned for the same user. Allowing more than one access key per user increases the attack surface by providing additional credentials that might be lost, leaked, or forgotten, making unauthorized access and credential compromise more likely if keys are not properly rotated or managed. To mitigate this risk, limit each IAM user to a single access key and revoke any unused or unnecessary keys to maintain strong security hygiene.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_access_key#user)

## Non-Compliant Code Examples
```aws
resource "aws_iam_access_key" "positive1" {
  user    = aws_iam_user.lb.name
  pgp_key = "keybase:some_person_that_exists"
}

resource "aws_iam_access_key" "positive2" {
  user    = aws_iam_user.lb.name
  pgp_key = "keybase:some_person_that_exists"
}


resource "aws_iam_user" "lb" {
  name = "loadbalancer"
  path = "/system/"
}

```

## Compliant Code Examples
```aws
resource "aws_iam_user" "userExample" {
  name = "loadbalancer"
  path = "/system/"

  tags = {
    tag-key = "tag-value"
  }
}

resource "aws_iam_access_key" "negative1" {
  user    = aws_iam_user.userExample.name
  pgp_key = "keybase:some_person_that_exists"
}


```