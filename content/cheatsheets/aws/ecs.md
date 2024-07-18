---
title: ECS
linktitle: ☸️ ECS
type: book
tags:
  - AWS
date: "2024-01-15T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 125
---

Container orchestration in AWS

<!--more-->

### Overview

Amazon Elastic Container Service is a highly scalable and high-performance container orchestration service.  You also don't have to worry about scheduling containers on those virtual machines. In addition, ECS is deeply integrated into the rest of the AWS ecosystem.

### ECS Cluster 

An Amazon ```ECS``` cluster is a logical grouping of ```tasks``` or ```services```.

* If you are running tasks or services that use the EC2 launch type, a cluster is also a grouping of container instances.
* If you are using Fargate, a cluster is also a logical grouping of Fargate capacity providers.
* A Cluster can be a combination of Fargate and EC2 launch types.

Task Definition: To prepare your application to run on Amazon ECS, you create a task definition. The task definition is a text file, in JSON format, that describes one or more containers, up to a maximum of ten, that form your application. It can be thought of as a blueprint for your application.

Task definitions are used to specify various parameters for your application. These parameters include which containers to use, which launch type to use, which ports should be opened for your application, and what data volumes should be used with the containers in the task. The specific parameters available for the task definition depend on which launch type you are using. 

Task: A task is the instantiation of a task definition within a cluster. After you have created a task definition for your application within Amazon ECS, you can specify the number of tasks that will run on your cluster.

Service: Amazon ECS allows you to run and maintain a specified number of instances of a task definition simultaneously in an Amazon ECS cluster. This is called a service. If any of your tasks should fail or stop for any reason, the Amazon ECS service scheduler launches another instance of your task definition to replace it and maintain the desired count of tasks in the service depending on the scheduling strategy used.

In addition to maintaining the desired count of tasks in your service, you can optionally run your service behind a load balancer. The load balancer distributes traffic across the tasks that are associated with the service. There are two service scheduler strategies available Replica  and Daemon .

{{% callout soon %}}
Coming soon...
{{% /callout %}}