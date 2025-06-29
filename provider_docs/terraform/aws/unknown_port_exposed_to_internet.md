---
title: "Unknown Port Exposed To Internet"
meta:
  name: "terraform/unknown_port_exposed_to_internet"
  id: "590d878b-abdc-428f-895a-e2b68a0e1998"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Networking and Firewall"
---
## Metadata
**Name:** `terraform/unknown_port_exposed_to_internet`
**Id:** `590d878b-abdc-428f-895a-e2b68a0e1998`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Networking and Firewall
## Description
This check identifies AWS Security Groups with ingress rules that allow traffic from the internet (0.0.0.0/0) on non-standard ports, which significantly increases your attack surface. When security groups allow uncommon ports to be accessed from anywhere on the internet, it exposes your resources to potential unauthorized access and exploitation from malicious actors globally. To secure your configuration, restrict ingress rules to specific IP ranges or use standard ports only, as shown in the example below:

```
ingress {
  from_port   = 443
  to_port     = 443
  protocol    = "tcp"
  cidr_blocks = ["192.168.0.0/24", "192.162.0.0/24"]
}
```

Avoid insecure configurations like: `cidr_blocks = ["0.0.0.0/0"]` for non-standard port ranges.

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group)

## Non-Compliant Code Examples
```aws
resource "aws_security_group" "positive1" {
  name        = "allow_tls"
  description = "Allow TLS inbound traffic"
  vpc_id      = aws_vpc.main.id

  ingress {
    description = "TLS from VPC"
    from_port   = 44
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "allow_tls"
  }
}

resource "aws_security_group" "positive2" {
  name        = "allow_tls"
  description = "Allow TLS inbound traffic"
  vpc_id      = aws_vpc.main.id

  ingress {
    description = "TLS from VPC"
    from_port   = 44
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["192.168.0.0/24", "0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["192.168.0.0/24", "0.0.0.0/0"]
  }

  tags = {
    Name = "allow_tls"
  }
}

```

## Compliant Code Examples
```aws
resource "aws_security_group" "negative1" {
  name        = "allow_tls"
  description = "Allow TLS inbound traffic"
  vpc_id      = aws_vpc.main.id

  ingress {
    description = "TLS from VPC"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["192.168.0.0/24", "192.162.0.0/24"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "allow_tls"
  }
}

```