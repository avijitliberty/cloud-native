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

There are 7 main sections will feature in the template and only the ```Resources``` section is **mandatory**.

1. **AWSTemplateFormatVersion**: This is the template version. As of now, we do have only “2010–09–09” as a version.
2. **Description**: This section will describe the CloudFormation template. We can use String to do that.
3. **Parameters**: This section contains collections of key-value pairs and it allows us to pass parameter values to the template. For an example, we can pass the instant type.
4. **Mappings**: This section contains collections of key-value pairs. We can define constant values in here and later we can refer those values. For an example, we can define our company public IPs.
5. **Conditions**: Conditions section contains conditional logic that define the circumstances under which entities are created or configured.
6. **Resources**: This section describes our AWS resources. This section is a **mandatory** section.
7. **Outputs**: In this section, we can define the values which need to take as an output.
For an example elastic IP or a load balancer DNS.

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
| * AllowedPattern          | * String                                      |
| * AllowedValues           | * Number                                      |
| * ConstraintDescription   | * List\<Number\>                                |
| * Default                 | * CommaDelimitedList                          |
| * Description             | * AWS Specific:                                |
| * MaxLength               | &emsp;&emsp;* AWS::EC2::Instance::Id                    |
| * MaxValue                | &emsp;&emsp;* AWS::EC2::VPC::Id                         |
| * MinLength               | &emsp;&emsp;* List\<AWS::EC2::Subnet::Id\>                |
| * MinValue                | &emsp;&emsp;* AWS::SSM::Parameter::Name                 |
| * NoEcho                  | &emsp;&emsp;* AWS::SSM::Parameter::Value                |
|                           | &emsp;&emsp;* AWS::SSM::Parameter::Value\<List\<String\>\>  |

### Mappings

{{< figure src="images/uploads/cloudformation-mappings.png" class="alignright">}}

* Mappings section matches a key to a corresponding set of named values. 
* For example, if we want to set values based on a region, we can create a mapping that uses region name as a key and contains the values we want to specify for each region
* We can use Fn::FindInMap intrinsic function to retrieve values in map.
* You can't include parameters, pseudo parameters, or intrinsic functions in the Mappings section.

```yml 
Mappings:
  RegionMap:
    us-east-1:
      HVM64: ami-079db87dc4c10ac91
    us-east-2:
      HVM64: ami-0ee4f2271a4df2d7d
    us-west-1:
      HVM64: ami-0082110c417e4726e   
    us-west-2:
      HVM64: ami-01450e8988a4e7f44    
```

{{% callout note %}}

**Interesting read**: https://stackoverflow.com/questions/44027960/cloudformation-possible-to-have-nested-mappings

{{% /callout %}}

### Conditions

* Conditions section contains statements that define the circumstances under which entities are created or configured.
  * **Example: 1** - We can create a condition and then associate it with a resource or output so that AWS CloudFormation only creates the resource or output if the condition is true.
  * **Example: 2** - We can associate the condition with a property so that AWS CloudFormation only sets the property to a specific value if the condition is true, if the condition is false, AWS CloudFormation sets the property to a different value that we specify.
* We will use conditions, when we want to re-use the template in different contexts like dev and prod environments. 
* Conditions are evaluated based on predefined Psuedo parameters or input parameter values that we specify when we create or update stack. 
* Within each condition we can reference the other condition. 
* We can associate these conditions in three places:
  * Resources
  * Resource Properties
  * Outputs 
* At stack creation or stack update, AWS CloudFormation evaluates all conditions in our template. During stack update, Resources that are now associated with a false condition are deleted. 
* We can use the below listed intrinsic functions to define conditions in cloud formation template. 
  * Fn::And
  * Fn::Equals
  * Fn::If
  * Fn::Not
  * Fn::Or

```yml
Conditions:
  CreateEIPForProd: !Equals [!Ref EnvironmentName, prod]
...
  ProdEIP:
    Type: AWS::EC2::EIP
    Condition: CreateEIPForProd
    Properties:
      InstanceId: !Ref VMInstance
```

{{% callout note %}}

**Important Note**: During stack update, we cannot update conditions by themselves. We can update conditions only when we include changes that add, modify or delete resources.

{{% /callout %}}
 
### Resources

* Resources are key components of a stack. 
* Resources section is a required section that need to be defined in cloud formation template.
* Syntax:
![CloudFormation-Resources](/images/uploads/cloudformation-resources.png)
* Resources Documentation:
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html

```yml
Resources:
  DevEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: ami-0082110c417e4726e
      InstanceType: t2.micro
      KeyName: cfn-key-1
      SecurityGroups:
        - !Ref SSHSecurityGroup
```

### EC2 User Data

* We can use UserData in CloudFormation template for ec2. 
* We need to use a intrinsic function Fn::Base64 with UserData in CFN templates. This function 
returns the Base64 representation of input string. It passes encoded data to ec2 Instance. 
* YAML Pipe (|): Any indented text that follows should be interpreted as a multi-line scalar value 
which means value should be interpreted literally in such a way that preserves newlines.
* UserData Cons:
  * By default, user data scripts and cloud-init directives run only during the boot cycle when 
  we first launch an instance.
  * We can update our configuration to ensure that our user data scripts and cloud-init directives run every time we restart our instance. (Reboot of server required)

```yml
  UserData:
    Fn::Base64:
     !Sub |
      #!/bin/bash -xe
      echo "Starting springboot-aws-starter"
      sudo su
      # install updates
      yum update -y
      # install java 8
      yum install java-1.8.0 -y
      # create the working directory
      mkdir /opt/springboot-aws-starter
      # download the maven artifact from S3
      aws s3 cp s3://cf-templates-us-west-1/springboot-aws-starter.jar /opt/springboot-aws-starter/ --region=us-west-1
      # change directories
      cd /opt/springboot-aws-starter
      # start application
      java -Ddb-instance-identifier=${DBInstanceIdentifier} -Ddatabase-name=${DBName} -DrdsUser=${DBUser} -DrdsPassword=${DBPassword} -Dspring.profiles.active=aws -jar springboot-aws-starter.jar
```

### Outputs

* Outputs section declares output values that we can 
  * Import in to other stacks (to create cross-stack references)
  * When using Nested stacks, outputs of a nested stack are used in Root Stack. 
* We can declare maximum of 60 outputs in a cfn template. 
* Export (Optional)
  * Exports contain resource output used for cross-stack reference. 
  * For each AWS account, Export name must be unique with in the region. As it 
  should be unique we can use the export name as “AWS::StackName”-ExportName
  * We can’t create cross-stack references across regions.
  * We can use the intrinsic function Fn::ImportValue to import values that have been 
  exported within the same region.  
  * For outputs, the value of the Name property of an Export can't use Ref or GetAtt
  functions that depend on a resource. 
  * We can’t delete a stack if another stack references one of its outputs. 
  * We can’t modify or remove an output value that is referenced by another stack. 
  * We can use Outputs in combination with Conditions. 

```yml
Outputs:
  JDBCConnectionString:
    Description: JDBC connection string for the database
    Value: !Join ['', ['jdbc:mysql://', !GetAtt [RDSInstance, Endpoint.Address], ':', !GetAtt [
          RDSInstance, Endpoint.Port], /, !Ref 'DBName']]
  ExternalUrl:
    Description: The url of the external load balancer
    Value: !Join ['', ['http://', !GetAtt 'LoadBalancer.DNSName','/context-root/']]
```

### Intrinsic Functions

* AWS CloudFormation provides several built-in functions that help you manage your stacks. 
* Use intrinsic functions to assign values to properties that are not available until runtime.
* Functions could be nested if need be.

There's a Long and Short format for each function:

* **Long form**: Fn::```FunctionName```: param
* **Short form**: !```FunctionName``` param

{{% callout note %}}

You can't nest two instances of two functions in short form. For example, the following syntax isn't valid:

```yml
!Base64 !Sub string
!Base64 !Ref logical_ID
```
Instead, use the full function name for at least one of the functions, as shown in the following examples:

```yml
!Base64
  "Fn::Sub": string
Fn::Base64:
  !Sub string
```
{{% /callout %}}

Following are a 🧠 list of most used functions:

###### !Ref

* The intrinsic function ```Ref``` returns the value of the specified parameter or resource. 
* **Resource** Case: When we specify a resource logical name, it returns a value that we can typically use to refer to that resource.
* **Parameter** Case: When we specify a parameter logical name, it returns the value of that parameter.
{{< figure src="images/uploads/cloudformation-ref-function.png" class="alignright">}}

* Syntax: 
  * Long Form: Ref: logicalName
  * Short Form: !Ref logicalName

###### !FindInMap

* The intrinsic function ```FindInMap``` returns the value corresponding to keys in a two-level map that is declared in Mappings section.
* Parameters
  * Map Name
  * Top Level Key
  * Second Level Key

###### !Equals

###### !And

###### !Or

###### !If

###### !Not

###### !GetAtt

###### !GetAZs

###### !Sub

###### !ImportValue

###### !Join

###### !Base64

###### !Select 

The intrinsic function ```!Select``` returns a single object from a list of objects by index.

```yml

Subnet0: 
  Type: "AWS::EC2::Subnet"
  Properties: 
    VpcId: !Ref VPC
    AvailabilityZone: !Select 
      - 0
      - Fn::GetAZs: !Ref 'AWS::Region'

```
{{% callout note %}}

**!Select** doesn't check for null values or if the index is out of bounds of the array. Both conditions will result in a stack error, so you should be certain that the index you choose is valid, and that the list contains non-null values.

{{% /callout %}}

### Pseudo Parameters

* Pseudo parameters are parameters that are predefined by AWS CloudFormation.
* We don’t need to declare them in our template.
* We can use them the same way as we use parameters as an argument for ```!Ref``` function.
{{< figure src="images/uploads/cloudformation-pseudo-params.png" class="alignright">}}

* Following are the pseudo parameters available: 
  * AWS::AccountId
  * AWS::NotificationARNs
  * AWS::NoValue
  * AWS::Partition
  * AWS::Region
  * AWS::StackId
  * AWS::StackName
  * AWS::URLSuffix

### Metadata

We have three types of metadata keys which are listed below: 
* Metadata Keys
  * AWS::CloudFormation::Designer: Auto generated during resources drag and drop to canvas.
  * AWS::CloudFormation::Interface: Used for parameter grouping.
  * AWS::CloudFormation::Init: Used for application installation and configurations on our aws compute (EC2 instances).

### Helper Scripts

### Nested Stacks

* The ```AWS::CloudFormation::Stack``` type nests a stack as a resource in a top-level template. 
* We can add output values from a nested stack within the root stack.
* We use ```Fn::GetAtt``` function with nested stacks logical name and the name of the output value in nested stack

```yml

  VPCStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      ...
      ...
  VPCId: !GetAtt VPCStack.Outputs.VpcId
  
```

