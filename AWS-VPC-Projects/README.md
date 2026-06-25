# AWS VPC NAT Gateway Project

## Project Overview

This project demonstrates the implementation of a secure AWS networking architecture using a Virtual Private Cloud (VPC), Public and Private Subnets, Internet Gateway, NAT Gateway, Route Tables, Security Groups, Network ACLs, and EC2 Instances.

The objective of this project is to provide internet access to a private EC2 instance through a NAT Gateway while preventing direct inbound internet access to the private instance.

---

## Architecture

### AWS Services Used

* Amazon VPC
* Public Subnet
* Private Subnet
* Internet Gateway (IGW)
* NAT Gateway
* Route Tables
* Security Groups
* Network ACLs (NACLs)
* Amazon EC2

---

## Network Configuration

### VPC

| Resource | CIDR Block   |
| -------- | ------------ |
| VPC      | 10.13.0.0/16 |

### Subnets

| Subnet         | CIDR Block    | Purpose                      |
| -------------- | ------------- | ---------------------------- |
| Public Subnet  | 10.13.0.0/20  | Bastion Host and NAT Gateway |
| Private Subnet | 10.13.16.0/20 | Private EC2 Instance         |

---

## Internet Gateway

An Internet Gateway was attached to the VPC to provide internet connectivity to resources located in the public subnet.

---

## NAT Gateway

A NAT Gateway was deployed in the public subnet with an Elastic IP address.

Purpose:

* Allow outbound internet access from private subnet resources.
* Prevent direct inbound internet access to private resources.

---

## Route Tables

### Public Route Table

| Destination  | Target           |
| ------------ | ---------------- |
| 10.13.0.0/16 | Local            |
| 0.0.0.0/0    | Internet Gateway |

Associated Subnet:

* Public Subnet (10.13.0.0/20)

### Private Route Table

| Destination  | Target      |
| ------------ | ----------- |
| 10.13.0.0/16 | Local       |
| 0.0.0.0/0    | NAT Gateway |

Associated Subnet:

* Private Subnet (10.13.16.0/20)

---

## EC2 Instances

### Public EC2 Instance (Bastion Host)

Purpose:

* Administrative access point.
* SSH access from local machine.
* Secure access to private EC2 instance.

### Private EC2 Instance

Purpose:

* Application server hosted inside private subnet.
* No direct internet exposure.
* Internet access provided through NAT Gateway.

---

## Security Groups

### Public EC2 Security Group

Inbound Rules:

| Type | Port | Source       |
| ---- | ---- | ------------ |
| SSH  | 22   | My Public IP |

Outbound Rules:

| Type        | Port | Destination |
| ----------- | ---- | ----------- |
| All Traffic | All  | 0.0.0.0/0   |

### Private EC2 Security Group

Inbound Rules:

| Type | Port | Source       |
| ---- | ---- | ------------ |
| SSH  | 22   | 10.13.0.0/16 |

Outbound Rules:

| Type        | Port | Destination |
| ----------- | ---- | ----------- |
| All Traffic | All  | 0.0.0.0/0   |

---

## Network ACL Configuration

### Private Subnet NACL

Inbound Rules:

| Rule No | Type       | Port Range | Source       | Action |
| ------- | ---------- | ---------- | ------------ | ------ |
| 98      | Custom TCP | 1024-65535 | 10.13.0.0/16 | Allow  |
| 99      | SSH        | 22         | 10.13.0.0/16 | Allow  |
| 100     | ICMP       | All        | 10.13.0.0/16 | Allow  |

Outbound Rules:

| Rule No | Type       | Port Range | Destination  | Action |
| ------- | ---------- | ---------- | ------------ | ------ |
| 98      | Custom TCP | 1024-65535 | 10.13.0.0/16 | Allow  |
| 99      | SSH        | 22         | 10.13.0.0/16 | Allow  |
| 120     | HTTP       | 80         | 0.0.0.0/0    | Allow  |
| 130     | HTTPS      | 443        | 0.0.0.0/0    | Allow  |

---

## Connectivity Validation

### SSH Access Validation

Successfully connected to the private EC2 instance through the public EC2 instance.

Flow:

Local Machine → Public EC2 → Private EC2

Command Used:

```bash
ssh -i ednit.pem ubuntu@10.x.x.x
```

---

### Internet Access Validation from Private EC2

Verified outbound internet connectivity using:

```bash
curl https://google.com
```

and

```bash
sudo apt update
```

Both commands executed successfully through the NAT Gateway.

---

## Key Learnings

* VPC Design and Implementation
* Public and Private Subnet Architecture
* Internet Gateway Configuration
* NAT Gateway Deployment
* Route Table Management
* Security Group Configuration
* Network ACL Configuration
* Bastion Host Architecture
* Secure SSH Connectivity
* Troubleshooting Network Connectivity Issues
* Providing Internet Access to Private Resources

---

## Project Outcome

Successfully designed and implemented a secure AWS VPC architecture where:

* Public resources access the internet through an Internet Gateway.
* Private resources access the internet through a NAT Gateway.
* Private EC2 instances remain inaccessible directly from the internet.
* Administrative access is controlled through a Bastion Host.
* Security Groups and NACLs enforce network security.
* Private instances can securely access the internet for software updates and package installation.
