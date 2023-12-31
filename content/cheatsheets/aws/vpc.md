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

 * private network to deploy your resources (regional resource)

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

### NACL

* A firewall which controls traffic from and to 
subnet
* Can have ALLOW and DENY rules
* Are attached at the Subnet level
* Rules only include IP addresses

### Security Groups

* A firewall that controls traffic to and from an ENI / an EC2 Instance
* Can have only ALLOW rules
* Rules include IP addresses and other security groups

 ![Network ACLs vs Security Groups](/images/uploads/nacl-vs-sg.png)

### ENI

### Auto Scaling

### VPC Endpoint



