---
title : "Verify the result"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

You had created a Pipeline to deploy your application to Amazon EKS in previous step. Let verify your Pipeline and make the changes on your source code to see the result.

### Verify Pipeline.
1. Go to [AWS CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1), you will see there is a Pipeline named **FCJ-CICD-EKS-CodePipeline** but the **Latest execution status** is **failed**. Click on your Pipeline to investigate the reason.

![Verify the result](../images/7.verifyresult/7.1.verifyresult.png?pc=90pt)

2. The failing on **Build** stage, click on **View details** for more information.
![Verify the result](../images/7.verifyresult/7.2.verifyresult.png?pc=90pt)

3. Scroll down to the end of log to see the failing by your EKS Cluster can not log in to ECR, since *no identity-based policy allows the ecr:GetAuthorizationToken action* on IAM Role **codebuild-FCJ-CICD-EKS-CodeBuildPrj-service-role**. Means CodeBuild not able to upload or push newly created Docker Image to ECR repository.
![Verify the result](../images/7.verifyresult/7.3.verifyresult.png?pc=90pt)

4. Now we need to attach which policy has **ecr:GetAuthorizationToken** action to IAM Role. Let go to [IAM Policies](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/policies) and find the policy named ```AmazonEC2ContainerRegistryPowerUser```, it has **ecr:GetAuthorizationToken** action so we can use it to attach to IAM Role.

![Verify the result](../images/7.verifyresult/7.4.verifyresult.png?pc=90pt)

5. Go to [IAM Roles](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/roles), find your IAM Role ```codebuild-FCJ-CICD-EKS-CodeBuildPrj-service-role```. Click on it.
![Verify the result](../images/7.verifyresult/7.5.verifyresult.png?pc=90pt)

6. Let attach the policy named ```AmazonEC2ContainerRegistryPowerUser``` to your IAM Role.
![Verify the result](../images/7.verifyresult/7.6.verifyresult.png?pc=90pt)

7. After attaching policy to your IAM Role successfully, go back to your [AWS CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines/FCJ-CICD-EKS-CodePipeline/view?region=ap-southeast-1). Click to **Retry failed actions**.
![Verify the result](../images/7.verifyresult/7.7.verifyresult.png?pc=90pt)

![Verify the result](../images/7.verifyresult/7.8.verifyresult.png?pc=90pt)


8. After **Build** step finished, the returned result is still fail. Let click on **View details** to see the reason.
![Verify the result](../images/7.verifyresult/7.9.verifyresult.png?pc=90pt)

9. The reason is CodeBuild do not have access to perform updates in EKS Cluster. Let create STS Assume Policy and Associate that to CodeBuild Role ```codebuild-FCJ-CICD-EKS-CodeBuildPrj-service-role```.

![Verify the result](../images/7.verifyresult/7.10.verifyresult.png?pc=90pt)

10. Go to [IAM Policies](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/policies). Click on **Create policy**.
![Verify the result](../images/7.verifyresult/7.11.verifyresult.png?pc=90pt)

11. At **Specify permissions** interface, config as the below parameters:
+ At **Service** field, select **STS**.
+ At **Actions allowed** field, select **AssumeRole** as **write** action.
+ At **Resources**, add your ARN.

12. Then click on **Next**.

![Verify the result](../images/7.verifyresult/7.12.verifyresult.png?pc=90pt)

13. At **Review and create** interface, input the value:
+ Input ```fcj-codebuild-sts-assume-role``` as **Policy name** .
+ Input ```CodeBuild to interact with EKS cluster to perform changes``` as **Description - optional**.

14. Then click on **Create policy**.
![Verify the result](../images/7.verifyresult/7.13.verifyresult.png?pc=90pt)

15. Go to [IAM Roles](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/roles/details/EksCodeBuildKubectlRole?section=permissions) and find your IAM Role named ```codebuild-FCJ-CICD-EKS-CodeBuildPrj-service-role```.
![Verify the result](../images/7.verifyresult/7.14.verifyresult.png?pc=90pt)

16. Attach our created policy on above step to this IAM Role.
![Verify the result](../images/7.verifyresult/7.15.verifyresult.png?pc=90pt)

17. Back to your Pipeline and click on **Retry stage** to re-run **Build Step**.
![Verify the result](../images/7.verifyresult/7.16.verifyresult.png?pc=90pt)

18. After a few minutes, your Pipeline was built successfully. 
![Verify the result](../images/7.verifyresult/7.17.verifyresult.png?pc=90pt)

### Verify the result.
1. Back to your Cloud9 Terminal, list all created Deployments, Services and Pods on your EKS Cluster.
```
kubectl get deploy,svc,pod -o wide
```
![Verify the result](../images/7.verifyresult/7.18.verifyresult.png?pc=90pt)

There are some created resources:
+ A created Deployment named **fcj-cicd-Deployment** with **Replicas** is **1** and **IMAGES** is <YOUR-ACCOUNT-ID>.dkr.ecr.ap-southeast-1.amazonaws.com/fcj-cicd-repository:<TAG-NAME> (In my case **IMAGES** is **170074558790.dkr.ecr.ap-southeast-1.amazonaws.com/fcj-cicd-repository:2024-05-21.20.59.18.1303bea5**).
+ A created Service named **fcj-cicd-nodeport-service** with **Type** is **NodePort** and **PORT** is **80:30000/TCP**.
+ A created Pod named **fcj-cicd-deployment-xxxxxxxxxx-xxxxx** with **STATUS** is **Running**.

2. Go to [ECR](https://ap-southeast-1.console.aws.amazon.com/ecr/private-registry/repositories?region=ap-southeast-1), click on your repository.
![Verify the result](../images/7.verifyresult/7.19.verifyresult.png?pc=90pt)

3. There is a created image on repository with **Image tag** match with <TAG-NAME> on **IMAGES** of **fcj-cicd-Deployment**.
![Verify the result](../images/7.verifyresult/7.20.verifyresult.png?pc=90pt)


4. Get **EXTERNAL-IP** of your Node.
```
kubectl get node -o wide
```
![Verify the result](../images/7.verifyresult/7.21.verifyresult.png?pc=90pt)

5. Access to your application URL ```http://<NODE's-EXTERNAL-IP>:30000```.
![Verify the result](../images/7.verifyresult/7.22.verifyresult.png?pc=90pt)

You can not access to your application because port 30000 of your Node is still not opened.

6. Go to your Security Group of your Node.
![Verify the result](../images/7.verifyresult/7.23.verifyresult.png?pc=90pt)

7. Click on **Edit inbound rules**.
![Verify the result](../images/7.verifyresult/7.24.verifyresult.png?pc=90pt)

8. Add a new rule:
+ **Type** is **Custom TCP**.
+ **Port range** is **30000**.
+ **Source** is **Anywhere - IPv4**.
9. Then, click on **Save rules**.
![Verify the result](../images/7.verifyresult/7.25.verifyresult.png?pc=90pt)

10. Let reload your application URL to verify the result.
![Verify the result](../images/7.verifyresult/7.26.verifyresult.png?pc=90pt)

Your application worked with version V1.

### Make changes to index.html.
1. Back to Cloud9 workspace, open file **index.html** inside **fcj-cicd-source/app**. Modify the code to version **V2** and save it.

![Verify the result](../images/7.verifyresult/7.27.verifyresult.png?pc=90pt)

2. Push your code to **AWS CodeCommit**.
```
git add .
git commit -m "Modify to version V2"
git push
```
![Verify the result](../images/7.verifyresult/7.28.verifyresult.png?pc=90pt)

3. Go to your [AWS CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines/FCJ-CICD-EKS-CodePipeline/view?region=ap-southeast-1) to verify that your Pipeline was triggered to run the process.
![Verify the result](../images/7.verifyresult/7.29.verifyresult.png?pc=90pt)

4. Wait to the Pipeline finish with status is success. Reload your application URL again to verify the result.
![Verify the result](../images/7.verifyresult/7.30.verifyresult.png?pc=90pt)
![Verify the result](../images/7.verifyresult/7.31.verifyresult.png?pc=90pt)

Your application was deployed to version V2.

#### Congratulations, you had deployed new version to your Amazon EKS Cluster with AWS CodePipeline successfully!

