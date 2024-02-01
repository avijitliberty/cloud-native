---
title: CICD
linktitle: CICD
type: book
tags:
  - AWS
date: "2019-05-05T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 130
---

CICD in AWS

<!--more-->

## Overview

CICD in AWS is composed of the following services. Most enterprises use most or some of them together to give the ```Continuous Integration and Continuous Delivery``` experience. 

- AWS CodeCommit – storing our code
- AWS CodePipeline – automating our pipeline from code to Elastic Beanstalk
- AWS CodeBuild – building and testing our code
- AWS CodeDeploy – deploying the code to EC2 instances (not Elastic Beanstalk)
- AWS CodeStar – manage software development activities in one place
- AWS CodeArtifact – store, publish, and share software packages
- AWS CodeGuru – automated code reviews using Machine Learning

![AWS-CICD](/images/uploads/cicd-techstack.png)

## CodeCommit

{{< figure src="images/uploads/codecommit-iam-policy.png" class="alignright">}}

-  Interactions are done using Git (standard)
-  Authentication:
    - SSH Keys – AWS Users can configure SSH keys in their IAM Console
    - HTTPS – with AWS CLI Credential helper or Git Credentials for IAM user
-  Authorization
    - IAM policies to manage users/roles permissions to repositories
-  Encryption
    - Repositories are automatically encrypted at rest using AWS KMS
    - Encrypted in transit (can only use HTTPS or SSH – both secure)
-  Cross-account Access
    - Do NOT share your SSH keys or your AWS credentials
    - Use an IAM Role in your AWS account and use AWS STS (AssumeRole API)
-  By default, a user who has push permissions to a CodeCommit repository can contribute to any branch
-  Use IAM policies to restrict users to push or merge code to a specific branch -  Example: only senior developers can push to production branch.
>  Note: ```Resource Policy``` is not supported yet.

- You can monitor CodeCommit events in ```EventBridge``` (near real-time). So anytime a pullRequestCreated, pullRequestStatusChanged, referenceCreated, commentOnCommitCreated...and so on you can react to that via EventBridge like so:
![CodeCommit Eventbridge](/images/uploads/codecommit-eventbridge.png)

- You can migrate a project hosted on another Git repository (e.g., Github, GitLab...) to CodeCommit repository
![CodeCommit Migration](/images/uploads/codecommit-migration.png)

![CodeCommit Vs GitHub](/images/uploads/codecommit-vs-github.png)

## CodeBuild

* A fully managed continuous integration (CI) service
* Continuous scaling (no servers to manage or provision – no build queue)
* Compile source code, run tests, produce software packages...
* Alternative to other build tools (e.g., Jenkins)
* Charged per minute for compute resources (time it takes to complete the builds)
* Leverages ```Docker``` under the hood for reproducible builds
* Use prepackaged Docker images or create your own custom Docker image
* Security:
  * Integration with ```KMS``` for encryption of build artifacts
  * IAM for CodeBuild permissions, and VPC for network security
  * AWS CloudTrail for API calls logging

* **Source** – CodeCommit, S3, Bitbucket, GitHub
* **Build instructions**: Code file ```buildspec.yml``` at the project root.
* **Output** logs can be stored in Amazon S3 & CloudWatch Logs
* Use CloudWatch Metrics to monitor build statistics
* Use EventBridge to detect failed builds and trigger notifications
* Use CloudWatch Alarms to notify if you need **thresholds** for failures
* Build Projects can be defined within CodePipeline or CodeBuild

###### How it Works
![CodeBuild – Operation](/images/uploads/codebuild-operation.png)

{{< figure src="images/uploads/buildspec.png" width="300" height="1000" class="alignright">}}

* **buildspec.yml** file must be at the root of your code 
* **env** – define environment variables 
  * variables – plaintext variables 
  * parameter-store – variables stored in SSM Parameter Store 
  * secrets-manager – variables stored in AWS Secrets Manager 
* **phases** – specify commands to run: 
  * install – install dependencies you may need for your build 
  * pre_build – final commands to execute before build 
  * Build – actual build commands 
  * post_build – finishing touches (e.g., zip output) 
* **artifacts** – what to upload to S3 (encrypted with KMS) 
* **cache** – files to cache (usually dependencies) to S3 for future build speedup

###### Environment Variables

* Default Environment Variables
  * Defined and provided by AWS
  * AWS_DEFAULT_REGION, CODEBUILD_BUILD_ARN, CODEBUILD_BUILD_ID, CODEBUILD_BUILD_IMAGE...
* Custom Environment Variables
  * Static – defined at build time (override using start-build API call)
  * Dynamic – using SSM Parameter Store and Secrets Manager

###### Inside VPC

* By default, your CodeBuild containers are launched outside your VPC 
* It cannot access resources in a VPC
* You can specify a VPC configuration: 
  * VPC ID 
  * Subnet IDs 
  * Security Group IDs 
* Then your build can access resources in your VPC (e.g., RDS, ElastiCache, EC2, ALB...)
* Use cases: integration tests, data query, internal load balancers..

###### Validate Pull Requests

* Validate proposed code changes in PRs before they get merged
* Ensure high level of code quality and avoid code conflicts

![CodeBuild – Validate Pull Requests](/images/uploads/codebuild-validate-pr.png)

###### Test Reports

{{< figure src="images/uploads/codebuild-test-reports.png" width="250" height="250" class="alignright">}}

* Contains details about tests that are run during builds
* Unit tests, configuration tests, functional tests
* Create your test cases with any test framework that can create report files in the following format:
  * JUnit XML, NUnit XML, NUnit3 XML
  * Cucumber JSON, TestNG XML, Visual Studio TRX
* Create a test report and add a Report Group name in ```buildspec.yml``` file with information about your tests

## CodeDeploy

## CodePipeline

-  Visual Workflow to orchestrate your CICD
-  **Source** – CodeCommit, ECR, S3, Bitbucket, GitHub
-  **Build** – CodeBuild, Jenkins, CloudBees, TeamCity
-  **Test** – CodeBuild, AWS Device Farm, 3rd party tools...
-  **Deploy** – CodeDeploy, Elastic Beanstalk, CloudFormation, ECS, S3...
-  **Invoke** – Lambda, Step Functions
-  Consists of stages:
    -  Each stage can have sequential actions and/or parallel actions
    -  Example: Build :arrow_right: Test :arrow_right: Deploy :arrow_right: Load Testing :arrow_right: …
    -  Manual approval can be defined at any stage

![Codepipeline – Artifacts](/images/uploads/codepipeline-artifacts.png)

With a blue/green deployment, you provision a new set of containers on which CodeDeploy  installs the latest version of your application. CodeDeploy then reroutes load balancer traffic from an existing set of containers running the previous version of your application to the new set of containers running the latest version. After traffic is rerouted to the new containers, the existing containers can be terminated. Blue/green deployments allow you to test the new application version before sending production traffic to it.

![Codepipeline – BG](/images/uploads/codepipeline-blue-green.png)

If there is an issue with the newly deployed application version, you can roll back to the previous version faster than with in-place deployments. 

## CodeArtifact

## CodeGuru