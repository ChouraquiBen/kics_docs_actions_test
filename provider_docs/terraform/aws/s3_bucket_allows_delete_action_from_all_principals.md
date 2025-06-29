---
title: "S3 Bucket Allows Delete Action From All Principals"
meta:
  name: "terraform/s3_bucket_allows_delete_action_from_all_principals"
  id: "ffdf4b37-7703-4dfe-a682-9d2e99bc6c09"
  cloud_provider: "terraform"
  severity: "CRITICAL"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/s3_bucket_allows_delete_action_from_all_principals`
**Id:** `ffdf4b37-7703-4dfe-a682-9d2e99bc6c09`
**Cloud Provider:** terraform
**Severity:** Critical
**Category:** Access Control
## Description
This vulnerability occurs when an S3 bucket policy allows the delete action from all principals (*), which can lead to unauthorized deletion of data and potential data loss or service disruption. Even when IP address conditions are applied, allowing delete actions from all principals presents a significant security risk as it could be exploited if the IP restriction is bypassed or misconfigured.

The insecure configuration looks like:
```
{
  "Effect": "Allow",
  "Principal": "*",
  "Action": "s3:DeleteObject",
  "Resource": "arn:aws:s3:::my_tf_test_bucket/*"
}
```

To secure your S3 bucket, either explicitly deny the action or restrict it to specific principals:
```
{
  "Effect": "Deny",
  "Action": "s3:*",
  "Resource": "arn:aws:s3:::my_tf_test_bucket/*"
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_policy)

## Non-Compliant Code Examples
```aws
resource "aws_s3_bucket_policy" "positive1" {
  bucket = aws_s3_bucket.b.id

  policy = <<POLICY
{
  "Version": "2012-10-17",
  "Id": "MYBUCKETPOLICY",
  "Statement": [
    {
      "Sid": "IPAllow",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::my_tf_test_bucket/*",
      "Condition": {
         "IpAddress": {"aws:SourceIp": "8.8.8.8/32"}
      }
    }
  ]
}
POLICY
}

```

```aws
module "s3_bucket" {
  source = "terraform-aws-modules/s3-bucket/aws"
  version = "3.7.0"

  bucket = "my-s3-bucket"
  acl    = "private"

  versioning = {
    enabled = true
  }

  policy = <<POLICY
{
  "Version": "2012-10-17",
  "Id": "MYBUCKETPOLICY",
  "Statement": [
    {
      "Sid": "IPAllow",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::my_tf_test_bucket/*",
      "Condition": {
         "IpAddress": {"aws:SourceIp": "8.8.8.8/32"}
      }
    }
  ]
}
POLICY
}

```

```aws
resource "aws_s3_bucket_policy" "positive2" {
  bucket = aws_s3_bucket.b.id

  policy = <<POLICY
{
  "Version": "2012-10-17",
  "Id": "MYBUCKETPOLICY",
  "Statement": [
    {
      "Sid": "IPAllow",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::my_tf_test_bucket/*",
      "Condition": {
         "IpAddress": {"aws:SourceIp": "8.8.8.8/32"}
      }
    }
  ]
}
POLICY
}

```

## Compliant Code Examples
```aws
resource "aws_s3_bucket_policy" "negative1" {
  bucket = aws_s3_bucket.b.id

  policy = <<POLICY
{
  "Version": "2012-10-17",
  "Id": "MYBUCKETPOLICY",
  "Statement": [
    {
      "Sid": "IPAllow",
      "Effect": "Deny",
      "Action": "s3:*",
      "Resource": "arn:aws:s3:::my_tf_test_bucket/*",
      "Condition": {
         "IpAddress": {"aws:SourceIp": "8.8.8.8/32"}
      }
    }
  ]
}
POLICY
}

```

```aws
module "s3_bucket" {
  source = "terraform-aws-modules/s3-bucket/aws"
  version = "3.7.0"

  bucket = "my-s3-bucket"
  acl    = "private"

  versioning = {
    enabled = true
  }

  policy = <<POLICY
{
  "Version": "2012-10-17",
  "Id": "MYBUCKETPOLICY",
  "Statement": [
    {
      "Sid": "IPAllow",
      "Effect": "Deny",
      "Action": "s3:*",
      "Resource": "arn:aws:s3:::my_tf_test_bucket/*",
      "Condition": {
         "IpAddress": {"aws:SourceIp": "8.8.8.8/32"}
      }
    }
  ]
}
POLICY
}

```