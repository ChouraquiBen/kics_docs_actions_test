---
title: "Remote Desktop Port Open To Internet"
meta:
  name: "terraform/remote_desktop_port_open_to_internet"
  id: "151187cb-0efc-481c-babd-ad24e3c9bc22"
  cloud_provider: "terraform"
  severity: "HIGH"
  category: "Networking and Firewall"
---
## Metadata
**Name:** `terraform/remote_desktop_port_open_to_internet`
**Id:** `151187cb-0efc-481c-babd-ad24e3c9bc22`
**Cloud Provider:** terraform
**Severity:** High
**Category:** Networking and Firewall
## Description
This check identifies AWS Security Groups that have Remote Desktop ports (commonly in ranges 3380-3450) exposed to the internet via '0.0.0.0/0' or similar CIDR blocks. Exposing Remote Desktop ports to the public internet creates a significant security risk as it allows potential attackers to attempt brute force attacks against your instances. Instead, restrict access to specific trusted IP ranges or use a bastion host/VPN for secure remote access.

Secure example:
```terraform
ingress {
  description = "Remote desktop open private"
  from_port   = 3380
  to_port     = 3450
  protocol    = "tcp"
  cidr_blocks = ["10.0.0.0/16"]  // Restricted to private network
}
```

Insecure example:
```terraform
ingress {
  description = "Remote desktop port open"
  from_port   = 3380
  to_port     = 3450
  protocol    = "tcp"
  cidr_blocks = ["0.0.0.0/0"]  // Open to the internet
}
```

#### Learn More

 - [Provider Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group)

## Non-Compliant Code Examples
```aws
resource "aws_security_group" "positive1" {
  name        = "rdp_positive_tcp_1"
  description = "Gets the remote desktop port open with the tcp protocol"

  ingress {
    description = "Remote desktop port open"
    from_port   = 3380
    to_port     = 3450
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_security_group" "positive2" {
  name        = "rdp_positive_tcp_2"
  description = "Gets the remote desktop port open with the tcp protocol"

  ingress {
    description = "Remote desktop port open"
    from_port   = 3381
    to_port     = 3445
    protocol    = "tcp"
    cidr_blocks = ["1.0.0.0/0"]
  }

  ingress {
    description = "Remote desktop port open"
    from_port   = 3000
    to_port     = 4000
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

```

## Compliant Code Examples
```aws
resource "aws_security_group" "negative1" {
  name        = "Dont open remote desktop port"
  description = "Doesn't enable the remote desktop port"

}

resource "aws_security_group" "negative2" {

  ingress {
    description = "Remote desktop open private"
    from_port   = 3380
    to_port     = 3450
    protocol    = "tcp"
  }
}

resource "aws_security_group" "negative_rdp_2" {

  ingress {
    description = "Remote desktop open private"
    from_port   = 3380
    to_port     = 3450
    protocol    = "tcp"
    cidr_blocks = ["0.1.0.0/0"]
  }
}

```