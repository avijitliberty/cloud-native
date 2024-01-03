---
title: VPC
linktitle: VPC
type: book
tags:
  - AWS
date: "2019-05-05T00:00:00+01:00"
# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 15
---

AWS VPC & Networking

<!--more-->

### Overview

Within a Amazon Virtual Private Cloud (Amazon VPC), you can launch AWS resources in a logically isolated virtual network that you've defined. This virtual network closely resembles a traditional network with the benefits of the scalable infrastructure of AWS.

### Regions

  ![AWS-Infrastructure](/images/uploads/aws-infrastructure.png)
 
 * AWS has Regions all around the world 
 * Names can be us-east-1, eu-west-3… 
 * A region is a cluster of data centers 
 * Most AWS services are region-scope
 * 👉 [AWS Global Infrastructure](https://infrastructure.aws/)

### Availability Zones

 * Each region has many availability zones. 
   Example:
    - ap-southeast-2a
    - ap-southeast-2b
    - ap-southeast-2c
* Each availability zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity
* They’re separate from each other, so that they’re isolated from disasters
* They’re connected with high bandwidth, ultra-low latency networking

### Edge Locations

 * Amazon has 400+ Points of Presence (400+ Edge Locations & 10+ Regional Caches) in 90+ cities across 40+ countries
 * Content is delivered to end users with lower latency

### VPC

 * Private network to deploy your resources (regional resource). 
 
 Here's how to create a VPC that you can use for a two-tier architecture in a production environment. To improve resiliency, you deploy the servers in two Availability Zones.

 ![AWS-VPC](/images/uploads/vpc-network.png)

### Subnets

Subnets allow you to partition your network inside your VPC (Availability Zone resource)
* A public subnet is a subnet that is accessible from the internet
* A private subnet is a subnet that is not accessible from the internet
* To define access to the internet and between subnets, we use Route Tables

### Route Tables

 * VPC has an implicit router, and you use route tables to control where network traffic is directed. 
 * Each subnet in your VPC must be associated with a route table, which controls the routing for the subnet.
 * A private subnet has a route defined to the NAT Gateway  for outbound traffic.
 * A public subnet has a route defined to the Internet Gateway for outbound traffic.
 * A subnet can only be associated with one route table at a time, but you can associate multiple subnets with the same subnet route table. 

### Internet Gateway

 * Internet Gateways helps our VPC instances connect with the internet
 * Public Subnets have a route to the internet gateway

### NAT Gateway

 * NAT Gateways (AWS-managed) & NAT Instances (self-managed) allow your instances in your Private Subnets 
to access the internet while remaining private

### Network Access Control List (NACL)

* A firewall which controls traffic from and to a subnet
* Can have ALLOW and DENY rules
* Are attached at the Subnet level
* Rules only include IP addresses
* Rules in the NACL are evaluated in order, starting with the lowest numbered rule. If traffic matches a rule, the rule is applied and no further rules are evaluated.
* NACLs are stateless, ingress does not equal egress. Traffic that matches a rule for one direction will not be automatically allowed in the opposite direction. You would have to add an outbound rule.

 ![NACL](/images/uploads/vpc-nacl.png)

 Here's an example of a NACL Outbound rule:

![NACL](/images/uploads/nacl-outbound.png)

{{% callout note %}}

 - Rule number 100 is denying the HTTP(80) traffic from all IPs.
 - Rule number 110 is allowing the HTTP(80) traffic from all IPs.

Rule evaluation starts from the lowest number, when there is matching rule found no futher evaluation happens. 
For this scenario, rule 100 is matched and denies all HTTP(80) traffic in the subnet.

{{% /callout %}}

### Security Groups

* A firewall that controls traffic to and from an ENI / an EC2 Instance
* You can specify the source, port range, and protocol for each inbound rule. 
* You can also specify the destination, port range, and protocol for each outbound rule.
* Can have only ALLOW rules. **Deny** is by default i.e anything not permitted is denied.
* Rules include IP addresses and other security groups
* All rules are evaluated to decide to allow or deny traffic. The most permissive rule is applied.
* Security Groups are stateful, ingress equals egress. Traffic that matches a rule for one direction will also be allowed automatically in the opposite direction.

 ![Security-Groups](/images/uploads/vpc-security-groups.png)

There are several key areas to compare between Security Groups and NACLs:

 | Area                       	| AWS Security Group                                                                                     	| AWS Network ACL                                                                                                                                                      	|
|----------------------------	|--------------------------------------------------------------------------------------------------------	|----------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| Scope                      	| Operates at the instance level                                                                         	| Operates at the subnet level                                                                                                                                         	|
| Association                	| Applies to an instance only if it is associated with the instance                                      	| Applies to all instances deployed in the associated subnet (providing an additional layer of defense if Security Group rules are too permissive)                     	|
| Quantity Limits            	| Instance can have multiple Security groups. The initial quota is 2500 per VPC with 60 rules per group. 	| Subnet can have only one NACL. The initial quota is 200 per VPC.                                                                                                     	|
| Destination                	| Destination can be a CIDR subnet, specific IP address, or another Security group                       	| Destination can only be a CIDR subnet,                                                                                                                               	|
| Implicit Deny              	| Implicit Deny. You can only add “allow” rules.                                                         	| Implicit Allow. You can add “allow” rules and “deny” rules                                                                                                           	|
| Evaluation Order and Logic 	| All rules are evaluated to decide to allow or deny traffic. The most permissive rule is applied.       	| Rules in the NACL are evaluated in order, starting with the lowest numbered rule. If traffic matches a rule, the rule is applied and no further rules are evaluated. 	|
| Stateful vs Stateless      	| Stateful: Ingress == Egress. Response traffic is allowed by default.                                   	| Stateless: Ingress != Egress. Return traffic must be explicitly allowed by rules                                                                                     	|

To visualize the stateful nature of AWS Security Groups vs the stateless nature of network ACLs here's a diagram:

![SecurityGroups-vs-NACL](/images/uploads/securitygroups-vs-nacl.png)

{{% callout note %}}

Without any outbound rules on the network ACL, traffic is blocked outbound, but with the outbound rule added, traffic flows correctly.

Whereas with the Security Group, because it is stateful, traffic is permitted outbound automatically once the inbound connection is established.

{{% /callout %}}


### Elastic Network Interface (ENI)

An elastic network interface is a logical networking component in a VPC that represents a virtual network card (NIC). It can include the following attributes:

{{< figure src="images/uploads/vpc-ec2-default-eni.png" width="300" height="300" class="alignright">}}

 - A primary private IPv4 address from the IPv4 address range of your VPC
 - A primary IPv6 address from the IPv6 address range of your VPC
 - One or more secondary private IPv4 addresses from the IPv4 address range of your VPC
 - One Elastic IP address (IPv4) per private IPv4 address
 - One public IPv4 address
 - One or more IPv6 addresses
 - One or more security groups
 - A MAC address
 - A source/destination check flag
 - A description

 Some of the key features of an ENI to remember:

 1. Each instance in your VPC has a default network interface (the primary network interface — eth0) that assigns a private IPv4 address from the IPv4 address range of your VPC.
 2. You cannot detach the default (primary) network interface from an instance.
 3. You can create an attach an additional/ secondary ENIs to an instance in your VPC. However, these ENIs should be created within the same availability zone of the EC2 instance that you are trying to attach your secondary ENI. The number of network instances you can attach varies by instance type.
 4. A **Security Group** is attached to an **ENI** not an **EC2** instance. With this approach you can have multiple routes to the same EC2 instance with different security configurations.
 5. You can create a network interface, attach it to an instance, detach it from an instance and attach it to another instance within the same Availability Zone. 
 6. A network interface’s attributes follow it as it is attached or detached from an instance and reattached to another instance. You could use this approach to change the publicIP of your instance if need be.

 ![Multiple-Eni](/images/uploads/vpc-ec2-multiple-eni.png)

### VPC Endpoint



