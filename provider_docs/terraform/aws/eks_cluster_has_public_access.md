---
title: "EKS Cluster Has Public Access"
meta:
  name: "terraform/eks_cluster_has_public_access"
  id: "42f4b905-3736-4213-bfe9-c0660518cda8"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Insecure Configurations"
---
## Metadata
**Name:** `terraform/eks_cluster_has_public_access`
**Id:** `42f4b905-3736-4213-bfe9-c0660518cda8`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Insecure Configurations
## Description
This check verifies that the `endpoint_public_access` attribute within the `vpc_config` block of an `aws_eks_cluster` resource is set to `false`. Allowing public access by setting `endpoint_public_access = true` exposes the EKS cluster's management endpoint to the internet, substantially increasing the risk of unauthorized access or potential attacks. Restricting the endpoint to private access ensures that only resources within the VPC can communicate with the EKS API, reducing the cluster's exposure and improving overall security.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eks_cluster)

## Non-Compliant Code Examples
```aws
resource "aws_eks_cluster" "positive1" {
  name     = "example"
  role_arn = aws_iam_role.example.arn

  vpc_config {
    subnet_ids = [aws_subnet.example1.id, aws_subnet.example2.id]
    endpoint_public_access = true
  }

  # Ensure that IAM Role permissions are created before and deleted after EKS Cluster handling.
  # Otherwise, EKS will not be able to properly delete EKS managed EC2 infrastructure such as Security Groups.
  depends_on = [
    aws_iam_role_policy_attachment.example-AmazonEKSClusterPolicy,
  ]
}

output "endpoint" {
  value = aws_eks_cluster.example.endpoint
}

output "kubeconfig-certificate-authority-data" {
  value = aws_eks_cluster.example.certificate_authority[0].data
}
```

## Compliant Code Examples
```aws
resource "aws_eks_cluster" "negative1" {
  name     = "example"
  role_arn = aws_iam_role.example.arn

  vpc_config {
    subnet_ids = [aws_subnet.example1.id, aws_subnet.example2.id]
    endpoint_public_access = false
  }

  # Ensure that IAM Role permissions are created before and deleted after EKS Cluster handling.
  # Otherwise, EKS will not be able to properly delete EKS managed EC2 infrastructure such as Security Groups.
  depends_on = [
    aws_iam_role_policy_attachment.example-AmazonEKSClusterPolicy,
  ]
}

output "endpoint" {
  value = aws_eks_cluster.example.endpoint
}

output "kubeconfig-certificate-authority-data" {
  value = aws_eks_cluster.example.certificate_authority[0].data
}
```