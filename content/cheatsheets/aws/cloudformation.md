---
title: CloudFormation
linktitle: CloudFormation
type: book
tags:
  - AWS
date: "2023-05-05T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 115
---

AWS infrastructure via CloudFormation

<!--more-->

### Overview

![AWS-CloudFormation](/images/uploads/cloudformation.png)

AWS CloudFormation is an AWS service which allows us to declaratively describe and provision almost all of the AWS services using the JSON or YAML format. It is defined as an “Infrastructure as a code”. That allows us to spend less time on managing infrastructure and more time spending on our application logic.

In AWS CloudFormation we will work with the CloudFormation template and using the template we are creating a stack. The template is written either by using JSON or YAML and it describes what AWS services are been provisioned into the stack. 

### Template Anatomy

There are 6 main sections will feature in the template and only the ```Resources``` section is **mandatory**.

1. **AWSTemplateFormatVersion**: This is the template version. As of now, we do have only “2010–09–09” as a version.
2. **Description**: This section will describe the CloudFormation template. We can use String to do that.
3. **Mappings**: This section contains collections of key-value pairs. We can define constant values in here and later we can refer those values.
For an example, we can define our company public IPs.
4. **Parameters**: This section contains collections of key-value pairs and it allows us to pass parameter values to the template.
For an example, we can pass the instant type.
5. **Resources**: This section describes our AWS resources. This section is a mandatory section.
6. **Outputs**: In this section, we can define the values which need to take as an output.
For an example elastic IP or a load balancer DNS.

### Intrinsic Functions

#### !Ref
* The intrinsic function ```Ref``` returns the value of the specified 
parameter or resource. 
* Resource Case: When we specify a resource logical name, it returns a value that we can typically use to refer to that resource.
* Parameter Case: When we specify a parameter logical name, it returns the value of that parameter.
{{< figure src="images/uploads/cloudformation-ref-function.png" class="alignright">}}

* Syntax: 
  * Long Form: Ref: logicalName
  * Short Form: !Ref logicalName

### Resources

* Resources are key components of a stack. 
* Resources section is a required section that need to be defined in cloud formation template.
* Syntax:
![CloudFormation-Resources](/images/uploads/cloudformation-resources.png)
* Resources Documentation:
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html

### Parameters

* Parameters enable us to input custom values to our template each time when we create or update stack.
* We can have maximum of **60** parameters in a cfn template.
* Each parameter must be given a logical name (logical id) which must be alphanumeric and unique among all logical names within the template.
* Each parameter must be assigned a **parameter type** that is supported by AWS CloudFormation. 
* Each parameter must be assigned a value at runtime for AWS CloudFormation to successfully provision the stack. We can optionally specify a default value for AWS CloudFormation to use unless another value is provided.
* Parameters must be declared and referenced within the same template. 
* We can reference parameters from the **Resources** and **Outputs** sections of the template.
* Syntax:
![CloudFormation-Parameters](/images/uploads/cloudformation-parameters.png)

* Here's a quick 🧠 for Parameter **properties** and **types**:

| **Parameter Properties ** | **Type \(Mandatory\) **                       |
|---------------------------|-----------------------------------------------|
| • AllowedPattern          | • String                                      |
| • AllowedValues           | • Number                                      |
| • ConstraintDescription   | • List\<Number\>                                |
| • Default                 | • CommaDelimitedList                          |
| • Description             | • AWS Specific                                |
| • MaxLength               | • AWS::EC2::Instance::Id                    |
| • MaxValue                | &emsp;&emsp;• AWS::EC2::VPC::Id                         |
| • MinLength               | &emsp;&emsp;• List\<AWS::EC2::Subnet::Id\>                |
| • MinValue                | &emsp;&emsp;• AWS::SSM::Parameter::Name                 |
| • NoEcho                  | &emsp;&emsp;• AWS::SSM::Parameter::Value                |
|                           | &emsp;&emsp;• AWS::SSM::Parameter::Value\<List\<String\>\>  |


