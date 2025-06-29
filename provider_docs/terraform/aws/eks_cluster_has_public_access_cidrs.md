---
title: "EKS Cluster Has Public Access CIDRs"
meta:
  name: "terraform/eks_cluster_has_public_access_cidrs"
  id: "61cf9883-1752-4768-b18c-0d57f2737709"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Networking and Firewall"
---
## Metadata
**Name:** `terraform/eks_cluster_has_public_access_cidrs`
**Id:** `61cf9883-1752-4768-b18c-0d57f2737709`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Networking and Firewall
## Description
Enabling the Amazon EKS public endpoint and allowing access from all IP addresses (0.0.0.0/0) exposes the Kubernetes cluster's API server to the entire internet. This configuration creates a significant security risk, as it allows unauthorized parties to attempt authentication or exploit vulnerabilities in the API server from anywhere in the world. If left unaddressed, this could lead to unauthorized access, data breaches, or disruption of workloads orchestrated by the cluster. Limiting public access to trusted IP address ranges greatly reduces the attack surface and helps safeguard sensitive operations and cluster resources.

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
    public_access_cidrs = ["0.0.0.0/0"]
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

resource "aws_eks_cluster" "positive2" {
  name     = "without_example"
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
```

## Compliant Code Examples
```aws
resource "aws_eks_cluster" "negative1" {
  name     = "example"
  role_arn = aws_iam_role.example.arn

  vpc_config {
    subnet_ids = [aws_subnet.example1.id, aws_subnet.example2.id]
    endpoint_public_access = true
    public_access_cidrs = ["1.1.1.1/1"]
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