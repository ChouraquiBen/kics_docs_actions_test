---
title: "Neptune Cluster With IAM Database Authentication Disabled"
meta:
  name: "terraform/neptune_cluster_with_iam_database_authentication_disabled"
  id: "c91d7ea0-d4d1-403b-8fe1-c9961ac082c5"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/neptune_cluster_with_iam_database_authentication_disabled`
**Id:** `c91d7ea0-d4d1-403b-8fe1-c9961ac082c5`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Access Control
## Description
AWS Neptune clusters should have IAM database authentication enabled to provide enhanced security by using IAM users and roles to authenticate to the database instead of standard username and password. When disabled, an attacker who gains access to the database credentials could directly connect to the database without additional IAM verification. To secure your Neptune cluster, add the 'iam_database_authentication_enabled = true' parameter as shown in the secure example:

```terraform
resource "aws_neptune_cluster" "example" {
  cluster_identifier = "neptune-cluster-demo"
  // other configuration...
  iam_database_authentication_enabled = true
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/neptune_cluster#storage_encrypted)

## Non-Compliant Code Examples
```aws
resource "aws_neptune_cluster" "positive1" {
  cluster_identifier                  = "neptune-cluster-demo"
  engine                              = "neptune"
  backup_retention_period             = 5
  preferred_backup_window             = "07:00-09:00"
  skip_final_snapshot                 = true
  apply_immediately                   = true
  storage_encrypted                   = true
}

resource "aws_neptune_cluster" "positive2" {
  cluster_identifier                  = "neptune-cluster-demo"
  engine                              = "neptune"
  backup_retention_period             = 5
  preferred_backup_window             = "07:00-09:00"
  skip_final_snapshot                 = true
  iam_database_authentication_enabled = false
  apply_immediately                   = true
  storage_encrypted                   = true
}

```

## Compliant Code Examples
```aws
resource "aws_neptune_cluster" "negative1" {
  cluster_identifier                  = "neptune-cluster-demo"
  engine                              = "neptune"
  backup_retention_period             = 5
  preferred_backup_window             = "07:00-09:00"
  skip_final_snapshot                 = true
  iam_database_authentication_enabled = true
  apply_immediately                   = true
  storage_encrypted                   = true
}

```