---
title : "Create CI/CD Pipeline to deploy application on Amazon EKS Cluster"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---
# Create CI/CD Pipeline to deploy application to Amazon EKS Cluster

### Overview

Continuous Integration and Continuous Deployment (CI/CD) pipelines are essential for automating the deployment of applications. This workshop outlines the steps required to set up a CI/CD pipeline for deploying applications to an Amazon Elastic Kubernetes Service (EKS) cluster.

![Create CI/CD Pipeline to deploy application to Amazon EKS](images/ekscicd.png?pc=60pt)

### Content

1. [Introduction](1-introduce/)
2. [Prerequisites](2-prerequiste/)
3. [Create Amazon ECR](3-interactwithecr/)
4. [Create AWS CodeCommit](4-createcodecommit/)
5. [Create STS Assume IAM Role for AWS CodeBuild](5-createstsiamrole/)
6. [Create AWS CodePipeline](6-createpipeline/)
7. [Verify the result](7-verifyresult/)
8. [Cleanup resources](8-cleanup/)