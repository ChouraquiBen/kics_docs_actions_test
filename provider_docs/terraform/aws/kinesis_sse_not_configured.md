---
title: "Kinesis SSE Not Configured"
meta:
  name: "terraform/kinesis_sse_not_configured"
  id: "5c6dd5e7-1fe0-4cae-8f81-4c122717cef3"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Encryption"
---
## Metadata
**Name:** `terraform/kinesis_sse_not_configured`
**Id:** `5c6dd5e7-1fe0-4cae-8f81-4c122717cef3`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Encryption
## Description
AWS Kinesis Firehose Delivery Streams should have Server-Side Encryption (SSE) properly configured to protect sensitive data at rest. Without encryption, data stored in Kinesis streams can be exposed to unauthorized access, potentially leading to data breaches and compliance violations. To secure Kinesis streams, the 'server_side_encryption' block must be included with 'enabled' set to true and a valid 'key_type' specified (either 'AWS_OWNED_CMK' or 'CUSTOMER_MANAGED_CMK' with corresponding 'key_arn').

Secure example:
```
resource "aws_kinesis_firehose_delivery_stream" "example" {
  // ... other configuration ...
  
  server_side_encryption {
    enabled  = true
    key_type = "CUSTOMER_MANAGED_CMK"
    key_arn  = "arn:aws:kms:region:account-id:key/key-id"
  }
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kinesis_firehose_delivery_stream#server_side_encryption)

## Non-Compliant Code Examples
```aws
resource "aws_kinesis_firehose_delivery_stream" "positive1" {
  name        = "${aws_s3_bucket.logs.bucket}-firehose"
  destination = "extended_s3"

  kinesis_source_configuration {
    kinesis_stream_arn = aws_kinesis_stream.cloudwatch-logs.arn
    role_arn           = aws_iam_role.firehose_role.arn
  }
}


resource "aws_kinesis_firehose_delivery_stream" "positive2" {
  name        = "${aws_s3_bucket.logs.bucket}-firehose"
  destination = "extended_s3"
}


resource "aws_kinesis_firehose_delivery_stream" "positive3" {
  name        = "${aws_s3_bucket.logs.bucket}-firehose"
  destination = "extended_s3"

  server_side_encryption {
    enabled = false
  }
}


resource "aws_kinesis_firehose_delivery_stream" "positive4" {
  name        = "${aws_s3_bucket.logs.bucket}-firehose"
  destination = "extended_s3"

  server_side_encryption {
    enabled  = true
    key_type = "AWS_OWN"
  }
}

resource "aws_kinesis_firehose_delivery_stream" "positive5" {
  name        = "${aws_s3_bucket.logs.bucket}-firehose"
  destination = "extended_s3"

  server_side_encryption {
    enabled  = true
    key_type = "CUSTOMER_MANAGED_CMK"
  }
}

```

## Compliant Code Examples
```aws

resource "aws_kinesis_firehose_delivery_stream" "negative1" {
  name        = "${aws_s3_bucket.logs.bucket}-firehose"
  destination = "extended_s3"

  server_side_encryption {
    enabled  = true
    key_type = "CUSTOMER_MANAGED_CMK"
    key_arn  = "qwewewre"
  }
}




resource "aws_kinesis_firehose_delivery_stream" "negative2" {
  name        = "${aws_s3_bucket.logs.bucket}-firehose"
  destination = "extended_s3"

  server_side_encryption {
    enabled  = true
    key_type = "AWS_OWNED_CMK"
  }
}


```