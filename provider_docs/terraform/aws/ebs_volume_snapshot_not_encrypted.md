---
title: "EBS Volume Snapshot Not Encrypted"
meta:
  name: "terraform/ebs_volume_snapshot_not_encrypted"
  id: "e6b4b943-6883-47a9-9739-7ada9568f8ca"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Encryption"
---
## Metadata
**Name:** `terraform/ebs_volume_snapshot_not_encrypted`
**Id:** `e6b4b943-6883-47a9-9739-7ada9568f8ca`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Encryption
## Description
EBS volume snapshots should be encrypted to protect sensitive data at rest from unauthorized access. When an EBS volume is unencrypted, snapshots derived from it remain unencrypted as well, potentially exposing sensitive information if accessed by malicious actors. The security impact includes potential data breaches, compliance violations, and unauthorized data access even if the original volume is no longer in use.

To ensure proper encryption, create your EBS volumes with encryption enabled as shown below:

```
resource "aws_ebs_volume" "secure_example" {
  availability_zone = "us-west-2a"
  size              = 40
  encrypted         = true

  tags = {
    Name = "HelloWorld"
  }
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/ebs_snapshot#encrypted)

## Non-Compliant Code Examples
```aws
resource "aws_ebs_volume" "positive1" {
  availability_zone = "us-west-2a"
  size              = 40
  encrypted         = false

  tags = {
    Name = "HelloWorld"
  }
}

resource "aws_ebs_snapshot" "positive1" {
  volume_id = aws_ebs_volume.positive1.id
  tags {
    Name = "Production"
  }
}

```

```aws
resource "aws_ebs_volume" "positive2" {
  availability_zone = "us-west-2a"
  size              = 40

  tags = {
    Name = "HelloWorld"
  }
}

resource "aws_ebs_snapshot" "positive2" {
  volume_id = aws_ebs_volume.positive2.id
  tags {
    Name = "Production"
  }
}

```

## Compliant Code Examples
```aws
resource "aws_ebs_volume" "negative1" {
  availability_zone = "us-west-2a"
  size              = 40
  encrypted         = true

  tags = {
    Name = "HelloWorld"
  }
}

resource "aws_ebs_snapshot" "negative1" {
  volume_id = aws_ebs_volume.negative1.id
  tags {
    Name = "Production"
  }
}

```