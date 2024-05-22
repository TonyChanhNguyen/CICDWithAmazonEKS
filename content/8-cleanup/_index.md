---
title : "Cleanup resources"
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 8. </b> "
---

### Delete AWS CodePipeline.
1. Go to [AWS CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1), select your Pipeline.
2. Click on **Delete pipeline**.

![Cleanup resources](../../images/8.cleanup/8.1.cleanup.png?pc=90pt)

3. Input ```delete``` to confirm.
4. Then, click on **Delete**.
![Cleanup resources](../../images/8.cleanup/8.2.cleanup.png?pc=90pt)


### Delete AWS CodeBuild Project.
1. Go to [Build projects](p-southeast-1.console.aws.amazon.com/codesuite/codebuild/projects?region=ap-southeast-1), select your project.
2. Click on **Action** and select **Delete**.
![Cleanup resources](../../images/8.cleanup/8.3.cleanup.png?pc=90pt)

3. Input ```delete``` to confirm and then click on **Delete**.
![Cleanup resources](../../images/8.cleanup/8.4.cleanup.png?pc=90pt)


### Delete AWS CodeCommit Repository.
1. Go to [AWS CodeCommit](https://ap-southeast-1.console.aws.amazon.com/codesuite/codecommit/repositories?region=ap-southeast-1), select your repository.
2. Click on **Delete repository**.
![Cleanup resources](../../images/8.cleanup/8.5.cleanup.png?pc=90pt)

3. Input ```delete``` to confirm and then click on **Delete**.
![Cleanup resources](../../images/8.cleanup/8.6.cleanup.png?pc=90pt)


### Delete Amazon ECR Repository.
1. Go to [ECR](https://ap-southeast-1.console.aws.amazon.com/ecr/home?region=ap-southeast-1), select your repository.
2. Click on **Delete**.
![Cleanup resources](../../images/8.cleanup/8.7.cleanup.png?pc=90pt)

3. Input ```delete``` to confirm and then click on **Delete**.
![Cleanup resources](../../images/8.cleanup/8.8.cleanup.png?pc=90pt)

### Delete S3 Bucket.
1. Go to [S3](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1#) and select bucket named **codepipeline-ap-southeast-1-xxxxxxxxxxxx**.
2. Click on **Empty**.
![Cleanup resources](../../images/8.cleanup/8.9.cleanup.png?pc=90pt)

3. Input ```permanently delete``` and click on **Empty**.
![Cleanup resources](../../images/8.cleanup/8.10.cleanup.png?pc=90pt)

4. Select your bucket again and click to **Delete**.

![Cleanup resources](../../images/8.cleanup/8.11.cleanup.png?pc=90pt)

5. Input the name of bucket to confirm, then click to **Delete bucket**.
![Cleanup resources](../../images/8.cleanup/8.12.cleanup.png?pc=90pt)

### Delete Amazon EKS Cluster
1. Back to Cloud9 Terminal, delete your EKS Cluster.
```
eksctl delete cluster --name fcj-cicd-cluster --region ap-southeast-1
```
![Cleanup resources](../../images/8.cleanup/8.13.cleanup.png?pc=90pt)

2. It will take you about 15 minutes to finish totally.
```
eksctl get cluster --region ap-southeast-1
```

![Cleanup resources](../../images/8.cleanup/8.14.cleanup.png?pc=90pt)

### Delete Cloud9 Workspace.
1. Go to [Cloud9](https://ap-southeast-1.console.aws.amazon.com/cloud9control/home?region=ap-southeast-1#/).
2. Select **FCJ-Workspace**.
3. Click on **Delete**.

![Cleanup resources](../../images/8.cleanup/8.15.cleanup.png?pc=90pt)

4. Input ```Delete``` to confirm.
5. Click on **Delete**.
![Cleanup resources](../../images/8.cleanup/8.16.cleanup.png?pc=90pt)


### Delete IAM Role.
Delete your created IAM Roles:
+ ```cwe-role-ap-southeast-1-FCJ-CICD-EKS-CodePipeline```
+ ```AWSCodePipelineServiceRole-ap-southeast-1-FCJ-CICD-EKS-CodePipe```
+ ```codebuild-FCJ-CICD-EKS-CodeBuildPrj-service-role```
+ ```EksCodeBuildKubectlRole```
![Cleanup resources](../../images/8.cleanup/8.18.cleanup.png?pc=90pt)


### Delete IAM Policy.
Delete your created IAM Policies:
+ ```fcj-codebuild-sts-assume-role```
+ ```start-pipeline-execution-ap-southeast-1-FCJ-CICD-EKS-CodePipeline```
+ ```AWSCodePipelineServiceRole-ap-southeast-1-FCJ-CICD-EKS-CodePipeline```
+ ```CodeBuildCloudWatchLogsPolicy-FCJ-CICD-EKS-CodeBuildPrj-ap-southeast-1```
+ ```CodeBuildBasePolicy-FCJ-CICD-EKS-CodeBuildPrj-ap-southeast-1```
![Cleanup resources](../../images/8.cleanup/8.17.cleanup.png?pc=90pt)