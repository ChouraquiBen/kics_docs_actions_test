---
title: "S3 Bucket Allows Put Action From All Principals"
meta:
  name: "terraform/s3_bucket_allows_put_action_from_all_principals"
  id: "d24c0755-c028-44b1-b503-8e719c898832"
  cloud_provider: "terraform"
  severity: "CRITICAL"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/s3_bucket_allows_put_action_from_all_principals`
**Id:** `d24c0755-c028-44b1-b503-8e719c898832`
**Cloud Provider:** terraform
**Severity:** Critical
**Category:** Access Control
## Description
When an S3 bucket policy allows PUT actions from all principals ('*'), it creates a significant security risk by permitting anyone on the internet to upload files to your bucket. This vulnerability can lead to data tampering, malicious file uploads, increased storage costs, and potentially serve as an attack vector for distributing malware or other harmful content through your infrastructure. To secure your S3 bucket, explicitly deny open PUT permissions as shown below, or restrict access to specific principals:

```hcl
// Insecure configuration
Statement: [
  {
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:PutObject"
  }
]

// Secure configuration
Statement: [
  {
    "Effect": "Deny",
    "Action": "s3:*"
  }
]
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
      "Action": "s3:PutObject",
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
      "Principal": {
        "AWS": "*"
      },
      "Action": "s3:PutObject",
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
      "Action": "s3:PutObject",
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