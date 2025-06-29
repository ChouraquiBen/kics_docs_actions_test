---
title: "ECS Task Definition Network Mode Not Recommended"
meta:
  name: "terraform/ecs_task_definition_network_mode_not_recommended"
  id: "9f4a9409-9c60-4671-be96-9716dbf63db1"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/ecs_task_definition_network_mode_not_recommended`
**Id:** `9f4a9409-9c60-4671-be96-9716dbf63db1`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Configurations
## Description
This check ensures that the `network_mode` attribute in an AWS ECS Task Definition is set to `awsvpc`. When `network_mode` is set to any value other than `awsvpc`, such as `none`, the tasks do not leverage the enhanced network security and isolation features provided by AWS VPCs. Without `awsvpc`, the container tasks may lack granular control over network traffic, security group assignment, and enforcement of network policies, making them more exposed to lateral movement and attacks within the cluster. If left unaddressed, this misconfiguration could lead to unauthorized access or unintended network exposure of container workloads, increasing the risk of compromise.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ecs_task_definition#network_mode)

## Non-Compliant Code Examples
```aws
resource "aws_ecs_task_definition" "positive1" {
  family                = "service"
  network_mode = "none"

  volume {
    name      = "service-storage"
    host_path = "/ecs/service-storage"
  }

  placement_constraints {
    type       = "memberOf"
    expression = "attribute:ecs.availability-zone in [us-west-2a, us-west-2b]"
  }
}
```

## Compliant Code Examples
```aws
resource "aws_ecs_task_definition" "negative1" {
  family                = "service"
  network_mode = "awsvpc"

  volume {
    name      = "service-storage"
    host_path = "/ecs/service-storage"
  }

  placement_constraints {
    type       = "memberOf"
    expression = "attribute:ecs.availability-zone in [us-west-2a, us-west-2b]"
  }
}
```