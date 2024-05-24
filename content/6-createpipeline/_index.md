---
title : "Create CodePipeline"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

1. Go to [CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1). Click on **Create pipeline**.

![Create CodePipeline](../images/6.createpipeline/6.1.createpipeline.png?pc=90pt)

2. At **Pipeline name** field, input the name of pipeline ```FCJ-CICD-EKS-CodePipeline```.

![Create CodePipeline](../images/6.createpipeline/6.2.createpipeline.png?pc=90pt)

3. Keep all another fields as default. Then, scroll down and click on **Next**.

![Create CodePipeline](../images/6.createpipeline/6.3.createpipeline.png?pc=90pt)

4. At **Add source stage** interface, config the below parameters:
+ At **Source provider**, select **AWS CodeCommit**.
+ At **Repository name**, select your created repository named **fcj-cicd-source**.
+ At **Branch name**, select **master** branch.
+ At **Change detection options**, choose the default (**Amazon CloudWatch Events (recommended)**).
+ At **Output artifact format**, choose the default (**CodePipeline default**).
5. Then, click on **Next**.
![Create CodePipeline](../images/6.createpipeline/6.4.createpipeline.png?pc=90pt)

6. At **Add build stage** interface, config the below parameters:
+ At **Build provider**, select **AWS CodeBuild**.
+ At **Region**, select **Asian Pacific (Singapore)**.

7. At **Project name**, click on **Create project**.

![Create CodePipeline](../images/6.createpipeline/6.5.createpipeline.png?pc=90pt)

8. At **Create build project** interface, input ```FCJ-CICD-EKS-CodeBuildPrj``` as **Project name** and keep default at another fields.
![Create CodePipeline](../images/6.createpipeline/6.6.createpipeline.png?pc=90pt)

9. Click to **Additional configuration** to expand more configurations.
![Create CodePipeline](../images/6.createpipeline/6.7.createpipeline.png?pc=90pt)

10. Add more **Environment variables** as below:
+ ```REPOSITORY_URI``` = ```<REPLACE-WITH-YOUR-ACCOUNT-ID>.dkr.ecr.ap-southeast-1.amazonaws.com/fcj-cicd-repository```
+ ```EKS_KUBECTL_ROLE_ARN``` = ```arn:aws:iam::<REPLACE-WITH-YOUR-ACCOUNT-ID>:role/EksCodeBuildKubectlRole```
+ ```EKS_CLUSTER_NAME``` = ```fcj-cicd-cluster```
![Create CodePipeline](../images/6.createpipeline/6.8.createpipeline.png?pc=90pt)


11. At **Build specifications** field, select **Use a buildspec file**.
![Create CodePipeline](../images/6.createpipeline/6.9.createpipeline.png?pc=90pt)

12. At **Logs** filed, input ```FCJ-CICD-EKS-CodeBuildPrj``` at **Group Name**.
13. Click on **Continue to CodePipeline**.
![Create CodePipeline](../images/6.createpipeline/6.10.createpipeline.png?pc=89pt)

14. You should be received the notification that your CodeBuild project created successfully.

15. Then, click on **Next**.

![Create CodePipeline](../images/6.createpipeline/6.11.createpipeline.png?pc=90pt)

16. At **Add deploy stage** interface, click on **Skip deploy stage** to skip this step.
17. Click **Skip** to confirm.
![Create CodePipeline](../images/6.createpipeline/6.12.createpipeline.png?pc=90pt)


18. At **Review** interface, let review your configuration. Then, click on **Create pipeline**.

![Create CodePipeline](../images/6.createpipeline/6.13.createpipeline.png?pc=90pt)

