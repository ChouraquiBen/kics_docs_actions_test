---
title: "EC2 Instance Using Default Security Group"
meta:
  name: "terraform/ec2_instance_using_default_security_group"
  id: "f1adc521-f79a-4d71-b55b-a68294687432"
  cloud_provider: "terraform"
  severity: "MEDIUM"
  category: "Access Control"
---
## Metadata
**Name:** `terraform/ec2_instance_using_default_security_group`
**Id:** `f1adc521-f79a-4d71-b55b-a68294687432`
**Cloud Provider:** terraform
**Severity:** Medium
**Category:** Access Control
## Description
Amazon EC2 instances should not use the default security group, as this group is open by default and permits unrestricted inbound and outbound traffic (`security_groups = [aws_security_group.default.id]`). Using the default security group increases the risk of unauthorized access, lateral movement, and exposure of sensitive workloads. To reduce this attack surface, instances should be assigned to tightly controlled, custom security groups with explicit ingress and egress rules.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#security_groups)

## Non-Compliant Code Examples
```aws
resource "aws_instance" "positive1" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"

  tags = {
    Name = "HelloWorld"
  }

  security_groups = [aws_security_group.default.id]
}

```

```aws
resource "aws_instance" "positive2" {
  ami = "ami-003634241a8fcdec0"

  instance_type = "t2.micro"

  vpc_security_group_ids = [aws_security_group.default.id]
}

```

## Compliant Code Examples
```aws
resource "aws_instance" "negative1" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"

  tags = {
    Name = "HelloWorld"
  }

  security_groups = [aws_security_group.sg.id]
}

```

```aws
resource "aws_instance" "negative2" {
  ami = "ami-003634241a8fcdec0"

  instance_type = "t2.micro"

  vpc_security_group_ids = [aws_security_group.sg.id]
}

```