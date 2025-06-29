---
title: "Elasticsearch Without IAM Authentication"
meta:
  name: "terraform/elasticsearch_without_iam_authentication"
  id: "e7530c3c-b7cf-4149-8db9-d037a0b5268e"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/elasticsearch_without_iam_authentication`
**Id:** `e7530c3c-b7cf-4149-8db9-d037a0b5268e`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
AWS Elasticsearch domains should enforce IAM authentication to restrict access to authorized users only. Without proper IAM policies—such as using `"Principal": "*"` in the `aws_elasticsearch_domain_policy` resource—unauthorized users could gain access to sensitive data by connecting from approved IP addresses. To mitigate this risk, the `Principal` field should be set to reference specific IAM principals, as shown below:

```
"Principal" : {
  "AWS": [
    "arn:aws:iam::123456789012:root",
    "arn:aws:iam::555555555555:root"
    ]
}
```

Failing to enforce IAM authentication can expose your Elasticsearch domain to unauthorized access and potential data breaches.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/elasticsearch_domain)

## Non-Compliant Code Examples
```aws
resource "aws_elasticsearch_domain" "example" {
  domain_name           = "tf-test"
  elasticsearch_version = "2.3"
}

resource "aws_elasticsearch_domain_policy" "main" {
  domain_name = aws_elasticsearch_domain.example.domain_name

  access_policies = <<POLICIES
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "es:*",
            "Principal": "*",
            "Effect": "Allow",
            "Condition": {
                "IpAddress": {"aws:SourceIp": "127.0.0.1/32"}
            },
            "Resource": "${aws_elasticsearch_domain.example.arn}/*"
        }
    ]
}
POLICIES
}

```

```aws
resource "aws_elasticsearch_domain" "example2" {
  domain_name           = "tf-test"
  elasticsearch_version = "2.3"
}

resource "aws_elasticsearch_domain_policy" "main2" {
  domain_name = aws_elasticsearch_domain.example2.domain_name

  access_policies = <<POLICIES
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "es:*",
            "Effect": "Allow",
            "Condition": {
                "IpAddress": {"aws:SourceIp": "127.0.0.1/32"}
            },
            "Resource": "${aws_elasticsearch_domain.example2.arn}/*"
        }
    ]
}
POLICIES
}

```

## Compliant Code Examples
```aws
resource "aws_elasticsearch_domain" "negativee" {
  domain_name           = "tf-test"
  elasticsearch_version = "2.3"
}

resource "aws_elasticsearch_domain_policy" "main8" {
  domain_name = aws_elasticsearch_domain.negativee.domain_name

  access_policies = <<POLICIES
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "es:*",
            "Principal" : {
              "AWS": [
                "arn:aws:iam::123456789012:root",
                "arn:aws:iam::555555555555:root"
                ]
            },
            "Effect": "Allow",
            "Condition": {
                "IpAddress": {"aws:SourceIp": "127.0.0.1/32"}
            },
            "Resource": "${aws_elasticsearch_domain.negativee.arn}/*"
        }
    ]
}
POLICIES
}

```