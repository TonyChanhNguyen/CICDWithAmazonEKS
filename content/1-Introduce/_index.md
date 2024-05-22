---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

### Overview

Continuous Integration and Continuous Deployment (CI/CD) pipelines are essential for automating the deployment of applications. This workshop outlines the steps required to set up a CI/CD pipeline for deploying applications to an Amazon Elastic Kubernetes Service (EKS) cluster.

![Create CI/CD Pipeline to deploy application to Amazon EKS](../images/ekscicd.png?pc=60pt)

+ [AWS CodePipeline](https://aws.amazon.com/codepipeline/?nc1=h_ls): is a fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates.
+ [AWS CodeCommit](https://aws.amazon.com/codecommit/?nc1=h_ls): is a secure, highly scalable, fully managed source control service that hosts private Git repositories.
+ [AWS CodeBuild](https://aws.amazon.com/codebuild/): is a fully managed continuous integration service that compiles source code, runs tests, and produces ready-to-deploy software packages.
+ [AWS ECR](https://aws.amazon.com/ecr/?nc1=h_ls): is a fully managed container registry offering high-performance hosting, so you can reliably deploy application images and artifacts anywhere.
+ [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls): is an object storage service offering industry-leading scalability, data availability, security, and performance. Millions of customers of all sizes and industries store, manage, analyze, and protect any amount of data for virtually any use case, such as data lakes, cloud-native applications, and mobile apps. With cost-effective storage classes and easy-to-use management features, you can optimize costs, organize and analyze data, and configure fine-tuned access controls to meet specific business and compliance requirements.

### Content

1. [Introduction](../1-introduce/)
2. [Prerequisites](../2-prerequiste/)
3. [Create Amazon ECR](../3-interactwithecr/)
4. [Create AWS CodeCommit](../4-createcodecommit/)
5. [Create STS Assume IAM Role for AWS CodeBuild](../5-createstsiamrole/)
6. [Create AWS CodePipeline](../6-createpipeline/)
7. [Verify the result](../7-verifyresult/)
8. [Cleanup resources](../8-cleanup/)
