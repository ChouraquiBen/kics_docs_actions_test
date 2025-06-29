---
title: "Elasticsearch with HTTPS disabled"
meta:
  name: "terraform/elasticsearch_with_https_disabled"
  id: "2e9e0729-66d5-4148-9d39-5e6fb4bf2a4e"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Networking and Firewall"
---
## Metadata
**Name:** `terraform/elasticsearch_with_https_disabled`
**Id:** `2e9e0729-66d5-4148-9d39-5e6fb4bf2a4e`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Networking and Firewall
## Description
Amazon Elasticsearch domains should enforce HTTPS by setting the `enforce_https` attribute to `true` in the `domain_endpoint_options` block. If `enforce_https` is left set to `false`, as shown below, communication with the Elasticsearch domain can occur over unencrypted HTTP, exposing data to interception and increasing the risk of man-in-the-middle attacks.

```
domain_endpoint_options {
  enforce_https = false
}
```

To mitigate this, always configure:

```
domain_endpoint_options {
  enforce_https = true
}
```

Enforcing HTTPS ensures that all data transmitted between clients and the Elasticsearch service is encrypted, protecting sensitive information from unauthorized access.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/elasticsearch_domain#enforce_https)

## Non-Compliant Code Examples
```aws
provider "aws" {
  region = "us-west-2"
}

resource "aws_elasticsearch_domain" "example" {
  domain_name           = "my-elasticsearch-domain"
  elasticsearch_version = "7.10"

  cluster_config {
    instance_type = "t2.small.elasticsearch"
    instance_count = 1
    dedicated_master_enabled = false
  }

  ebs_options {
    ebs_enabled = true
    volume_type = "gp2"
    volume_size = 10
  }

  vpc_options {
    subnet_ids         = ["subnet-xxxxxxxx", "subnet-yyyyyyyy"]
    security_group_ids = ["sg-xxxxxxxx"]
  }

  domain_endpoint_options {
    enforce_https = false
  }

  tags = {
    Name        = "my-elasticsearch-domain"
    Environment = "production"
  }
}

```

## Compliant Code Examples
```aws
provider "aws" {
  region = "us-west-2"
}

resource "aws_elasticsearch_domain" "example" {
  domain_name           = "my-elasticsearch-domain"
  elasticsearch_version = "7.10"

  cluster_config {
    instance_type = "t2.small.elasticsearch"
    instance_count = 1
    dedicated_master_enabled = false
  }

  ebs_options {
    ebs_enabled = true
    volume_type = "gp2"
    volume_size = 10
  }

  vpc_options {
    subnet_ids         = ["subnet-xxxxxxxx", "subnet-yyyyyyyy"]
    security_group_ids = ["sg-xxxxxxxx"]
  }

  domain_endpoint_options {
    enforce_https = true
  }

  tags = {
    Name        = "my-elasticsearch-domain"
    Environment = "production"
  }
}

```