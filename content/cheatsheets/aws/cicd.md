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

### CodeCommit

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
-  Note: ```Resource Policy``` is not supported yet.

![CodeCommit Vs GitHub](/images/uploads/codecommit-vs-github.png)

### CodePipeline

-  Visual Workflow to orchestrate your CICD
-  Source – CodeCommit, ECR, S3, Bitbucket, GitHub
-  Build – CodeBuild, Jenkins, CloudBees, TeamCity
-  Test – CodeBuild, AWS Device Farm, 3rd party tools...
-  Deploy – CodeDeploy, Elastic Beanstalk, CloudFormation, ECS, S3...
-  Invoke – Lambda, Step Functions
-  Consists of stages:
    -  Each stage can have sequential actions and/or parallel actions
    -  Example: Build èTest è Deploy è Load Testing è …
    -  Manual approval can be defined at any stage
