---
title: "S3 Static Website Host Enabled"
meta:
  name: "terraform/s3_static_website_host_enabled"
  id: "42bb6b7f-6d54-4428-b707-666f669d94fb"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/s3_static_website_host_enabled`
**Id:** `42bb6b7f-6d54-4428-b707-666f669d94fb`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Insecure Configurations
## Description
AWS S3 static website hosting allows serving content directly from buckets without additional authentication, potentially exposing sensitive data to the internet. When enabled via the 'website' block, the bucket content becomes accessible through the website endpoint, bypassing S3's access controls and increasing attack surface. Attackers could access unintended data if bucket policies are misconfigured or files are incorrectly permissioned.

Secure configuration example:
```
resource "aws_s3_bucket" "secure_example" {
  bucket = "s3-website-test.hashicorp.com"
  acl    = "public-read"
  // No website configuration block
}
```

Instead, consider using CloudFront distribution with proper access controls and HTTPS to securely serve website content.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket#website)

## Non-Compliant Code Examples
```aws
provider "aws" {
  region = "us-east-1"
}

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
}

resource "aws_s3_bucket" "positive1" {
  bucket = "s3-website-test.hashicorp.com"
  acl    = "public-read"

  website {
    index_document = "index.html"
    error_document = "error.html"
  }
}

```

```aws
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "4.2.0"
    }
  }
}

provider "aws" {
  # Configuration options
}


resource "aws_s3_bucket" "buc" {
  bucket = "my-tf-test-bucket"

  tags = {
    Name        = "My bucket"
    Environment = "Dev"
  }
}

resource "aws_s3_bucket_website_configuration" "example" {
  bucket = aws_s3_bucket.buc.bucket

  index_document {
    suffix = "index.html"
  }

  error_document {
    key = "error.html"
  }

  routing_rule {
    condition {
      key_prefix_equals = "docs/"
    }
    redirect {
      replace_key_prefix_with = "documents/"
    }
  }
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

  website {
    index_document = "index.html"
    error_document = "error.html"
  }
}

```

## Compliant Code Examples
```aws
provider "aws" {
  region = "us-east-1"
}

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
}

resource "aws_s3_bucket" "negative1" {
  bucket = "s3-website-test.hashicorp.com"
  acl    = "public-read"
}

```

```aws
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "4.2.0"
    }
  }
}

provider "aws" {
  # Configuration options
}


resource "aws_s3_bucket" "bu" {
  bucket = "my-tf-test-bucket"

  tags = {
    Name        = "My bucket"
    Environment = "Dev"
  }
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
}

```