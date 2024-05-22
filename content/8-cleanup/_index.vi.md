---
title : "Dọn dẹp tài nguyên"
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 8. </b> "
---

### Xóa AWS CodePipeline.
1. Đi đến [AWS CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1), chọn Pipeline của bạn.
2. Nhấn **Delete pipeline**.

![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.1.cleanup.png?pc=90pt)

3. Nhập ```delete``` để xác nhận.
4. Sau đó, nhấn **Delete**.
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.2.cleanup.png?pc=90pt)


### Xóa AWS CodeBuild Project.
1. Đi đến [Build projects](p-southeast-1.console.aws.amazon.com/codesuite/codebuild/projects?region=ap-southeast-1), chọn project của bạn.
2. Nhấn **Action** và chọn **Delete**.
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.3.cleanup.png?pc=90pt)

3. Nhập ```delete``` để xác nhận và sau đó nhấn **Delete**.
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.4.cleanup.png?pc=90pt)


### Xóa AWS CodeCommit Repository.
1. Đi đến [AWS CodeCommit](https://ap-southeast-1.console.aws.amazon.com/codesuite/codecommit/repositories?region=ap-southeast-1), chọn repository của bạn.
2. Nhấn **Delete repository**.
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.5.cleanup.png?pc=90pt)

3. Nhập ```delete``` để xác nhận và sau đó nhấn **Delete**.
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.6.cleanup.png?pc=90pt)


### Xóa Amazon ECR Repository.
1. Đi đesn [ECR](https://ap-southeast-1.console.aws.amazon.com/ecr/home?region=ap-southeast-1), chọn repository của bạn.
2. Nhấn **Delete**.
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.7.cleanup.png?pc=90pt)

3. Nhập ```delete``` để xác nhận và sau đó nhấn **Delete**.
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.8.cleanup.png?pc=90pt)

### Xóa S3 Bucket.
1. Đi đến [S3](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1#) và chọn bucket tên **codepipeline-ap-southeast-1-xxxxxxxxxxxx**.
2. Nhấn **Empty**.
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.9.cleanup.png?pc=90pt)

3. Nhập ```permanently delete``` và nhấn **Empty**.
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.10.cleanup.png?pc=90pt)

4. Chọn lại bucket và nhấn **Delete**.

![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.11.cleanup.png?pc=90pt)

5. Nhập tên bucket để xác nhận, sau đó nhấn **Delete bucket**.
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.12.cleanup.png?pc=90pt)

### Xóa Amazon EKS Cluster
1. Trở lại cửa sổ lệnh Cloud9, xóa EKS Cluster.
```
eksctl delete cluster --name fcj-cicd-cluster --region ap-southeast-1
```
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.13.cleanup.png?pc=90pt)

2. Mất khoảng 15 phút để Cluster được xóa hoàn toàn
```
eksctl get cluster --region ap-southeast-1
```

![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.14.cleanup.png?pc=90pt)

### Xóa Cloud9 Workspace.
1. Đi đến [Cloud9](https://ap-southeast-1.console.aws.amazon.com/cloud9control/home?region=ap-southeast-1#/).
2. Chọn **FCJ-Workspace**.
3. Nhấn **Delete**.

![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.15.cleanup.png?pc=90pt)

4. Nhập ```Delete``` để xác nhận.
5. Nhấn **Delete**.
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.16.cleanup.png?pc=90pt)


### Xóa IAM Role.
Xóa các IAM Role đã tạo:
+ ```cwe-role-ap-southeast-1-FCJ-CICD-EKS-CodePipeline```
+ ```AWSCodePipelineServiceRole-ap-southeast-1-FCJ-CICD-EKS-CodePipe```
+ ```codebuild-FCJ-CICD-EKS-CodeBuildPrj-service-role```
+ ```EksCodeBuildKubectlRole```
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.18.cleanup.png?pc=90pt)


### Xóa IAM Policy.
Xóa các IAM Policy đã tạo:
+ ```fcj-codebuild-sts-assume-role```
+ ```start-pipeline-execution-ap-southeast-1-FCJ-CICD-EKS-CodePipeline```
+ ```AWSCodePipelineServiceRole-ap-southeast-1-FCJ-CICD-EKS-CodePipeline```
+ ```CodeBuildCloudWatchLogsPolicy-FCJ-CICD-EKS-CodeBuildPrj-ap-southeast-1```
+ ```CodeBuildBasePolicy-FCJ-CICD-EKS-CodeBuildPrj-ap-southeast-1```
![Dọn dẹp tài nguyên](../../../images/8.cleanup/8.17.cleanup.png?pc=90pt)