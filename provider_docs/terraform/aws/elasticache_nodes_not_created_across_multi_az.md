---
title: "ElastiCache Nodes Not Created Across Multi AZ"
meta:
  name: "terraform/elasticache_nodes_not_created_across_multi_az"
  id: "6db03a91-f933-4f13-ab38-a8b87a7de54d"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Availability"
---
## Metadata
**Name:** `terraform/elasticache_nodes_not_created_across_multi_az`
**Id:** `6db03a91-f933-4f13-ab38-a8b87a7de54d`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Availability
## Description
When configuring AWS ElastiCache clusters with multiple nodes, it is important to distribute nodes across multiple Availability Zones (AZs) by setting the `az_mode` attribute to `"cross-az"`. Failing to do so—such as by omitting the `az_mode` attribute or explicitly setting it to `"single-az"`—means all nodes run in a single AZ, increasing the risk of service disruption if that AZ experiences an outage. For example, a secure Terraform configuration would explicitly set:

```
resource "aws_elasticache_cluster" "example" {
  cluster_id    = "cluster-example"
  engine        = "memcached"
  num_cache_nodes = 3
  az_mode       = "cross-az"
}
```

Distributing cache nodes across multiple AZs increases fault tolerance and availability for ElastiCache clusters.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/elasticache_cluster)

## Non-Compliant Code Examples
```aws
resource "aws_elasticache_cluster" "positive1" {
  cluster_id = "cluster-example"
  engine = "memcached"
  num_cache_nodes = 3
}

resource "aws_elasticache_cluster" "positive2" {
  cluster_id = "cluster-example"
  engine = "memcached"
  num_cache_nodes = 3

  az_mode = "single-az"
}
```

## Compliant Code Examples
```aws
resource "aws_elasticache_cluster" "negative1" {
  cluster_id           = "cluster-example"
  engine = "memcached"

  num_cache_nodes = 3

  az_mode = "cross-az"
}
```