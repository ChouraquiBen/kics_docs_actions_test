---
title: "IAM Policy Grants 'AssumeRole' Permission Across All Services"
meta:
  name: "terraform/iam_policy_grants_assumerole_permission_across_all_services"
  id: "bcdcbdc6-a350-4855-ae7c-d1e6436f7c97"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/iam_policy_grants_assumerole_permission_across_all_services`
**Id:** `bcdcbdc6-a350-4855-ae7c-d1e6436f7c97`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
IAM Policies should not grant the 'AssumeRole' permission across all services by using wildcard principals, such as `"AWS": "*"`. Allowing any AWS account or principal to assume a role poses a significant security risk, as it removes any restriction over who can gain the role's permissions. If left unaddressed, this misconfiguration could enable unauthorized users or malicious actors to assume sensitive roles and gain elevated privileges within your AWS environment, resulting in potential data loss, privilege escalation, or compromise of critical cloud resources. Restricting the principals allowed to assume a role to only trusted services or accounts is essential for maintaining a secure cloud infrastructure.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role)

## Non-Compliant Code Examples
```aws
//  Create a role which OpenShift instances will assume.
//  This role has a policy saying it can be assumed by ec2
//  instances.
resource "aws_iam_role" "positive1" {
  name = "${var.name_tag_prefix}-openshift-instance-role"

  assume_role_policy = <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "sts:AssumeRole",
            "Principal": {
                "Service": "ec2.amazonaws.com",
                "AWS": "*"
            },
            "Effect": "Allow",
            "Sid": ""
        }
    ]
}
EOF
}

//  This policy allows an instance to forward logs to CloudWatch, and
//  create the Log Stream or Log Group if it doesn't exist.
resource "aws_iam_policy" "positive3" {
  name        = "${var.name_tag_prefix}-openshift-instance-forward-logs"
  path        = "/"
  description = "Allows an instance to forward logs to CloudWatch"

  policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogStreams"
    ],
      "Resource": [
        "arn:aws:logs:*:*:*"
    ]
  }
 ]
}
EOF
}


//  Attach the policies to the role.
resource "aws_iam_policy_attachment" "positive4" {
  name       = "${var.name_tag_prefix}-openshift-attachment-forward-logs"
  roles      = ["${aws_iam_role.openshift-instance-role.name}"]
  policy_arn = "aws_iam_policy.openshift-policy-forward-logs.arn"
}

//  Create a instance profile for the role.
resource "aws_iam_instance_profile" "positive5" {
  name  = "${var.name_tag_prefix}-openshift-instance-profile"
  role = "aws_iam_role.openshift-instance-role.name"
}

resource "aws_iam_role" "positive2" {
  name = "${var.name_tag_prefix}-openshift-instance-role"

  assume_role_policy = <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "sts:AssumeRole",
            "Principal": {
                "Service": "ec2.amazonaws.com",
                "AWS": "*"
            },
            "Sid": ""
        }
    ]
}
EOF
}

```

## Compliant Code Examples
```aws
//  Create a role which OpenShift instances will assume.
//  This role has a policy saying it can be assumed by ec2
//  instances.
resource "aws_iam_role" "negative1" {
  name = "${var.name_tag_prefix}-openshift-instance-role"

  assume_role_policy = <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "sts:AssumeRole",
            "Principal": {
                "Service": "ec2.amazonaws.com"
            },
            "Effect": "Allow",
            "Sid": ""
        }
    ]
}
EOF
}

//  This policy allows an instance to forward logs to CloudWatch, and
//  create the Log Stream or Log Group if it doesn't exist.
resource "aws_iam_policy" "negative2" {
  name        = "${var.name_tag_prefix}-openshift-instance-forward-logs"
  path        = "/"
  description = "Allows an instance to forward logs to CloudWatch"

  policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogStreams"
    ],
      "Resource": [
        "arn:aws:logs:*:*:*"
    ]
  }
 ]
}
EOF
}


//  Attach the policies to the role.
resource "aws_iam_policy_attachment" "negative3" {
  name       = "${var.name_tag_prefix}-openshift-attachment-forward-logs"
  roles      = ["${aws_iam_role.openshift-instance-role.name}"]
  policy_arn = "aws_iam_policy.openshift-policy-forward-logs.arn"
}

//  Create a instance profile for the role.
resource "aws_iam_instance_profile" "negative4" {
  name  = "${var.name_tag_prefix}-openshift-instance-profile"
  role = "aws_iam_role.openshift-instance-role.name"
}

```