---
title : "Create STS Assume IAM Role for AWS CodeBuild"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
In an AWS CodePipeline, we are going to use AWS CodeBuild to deploy changes to our Kubernetes manifests.
This requires an AWS IAM role capable of interacting with the EKS cluster.
In this step, we are going to create an IAM role and add an inline policy EKS:Describe that we will use in the CodeBuild stage to interact with the EKS cluster via kubectl

### Create IAM Role
1. At Cloud9 terminal, export your Account ID.
```
export ACCOUNT_ID=<REPLACE-WITH-YOUR-ACCOUNT-ID>
```
![Create STS Assume IAM Role for AWS CodeBuild](../images/5.createstsassumerole/5.1.createstsassumerole.png?pc=90pt)

2. Set Trust Policy.
```
TRUST="{ \"Version\": \"2012-10-17\", \"Statement\": [ { \"Effect\": \"Allow\", \"Principal\": { \"AWS\": \"arn:aws:iam::${ACCOUNT_ID}:root\" }, \"Action\": \"sts:AssumeRole\" } ] }"
echo $TRUST
```
![Create STS Assume IAM Role for AWS CodeBuild](../images/5.createstsassumerole/5.2.createstsassumerole.png?pc=90pt)

3. Create IAM Role for CodeBuild to Interact with EKS.
```
aws iam create-role --role-name EksCodeBuildKubectlRole --assume-role-policy-document "$TRUST" --output text --query 'Role.Arn'
```
![Create STS Assume IAM Role for AWS CodeBuild](../images/5.createstsassumerole/5.3.createstsassumerole.png?pc=90pt)

4. Define Inline Policy with eks Describe permission in a file iam-eks-describe-policy.
```
echo '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": "eks:Describe*", "Resource": "*" } ] }' > /tmp/iam-eks-describe-policy
```
![Create STS Assume IAM Role for AWS CodeBuild](../images/5.createstsassumerole/5.4.createstsassumerole.png?pc=90pt)

5. Associate Inline Policy to our newly created IAM Role.
```
aws iam put-role-policy --role-name EksCodeBuildKubectlRole --policy-name eks-describe --policy-document file:///tmp/iam-eks-describe-policy
```
![Create STS Assume IAM Role for AWS CodeBuild](../images/5.createstsassumerole/5.5.createstsassumerole.png?pc=90pt)

6. Go to [IAM Role](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/roles) to verify the created Role named ```EksCodeBuildKubectlRole```.
![Create STS Assume IAM Role for AWS CodeBuild](../images/5.createstsassumerole/5.6.createstsassumerole.png?pc=90pt)


### Update EKS Cluster aws-auth ConfigMap with created Role.
1. Verify what is present in aws-auth configmap before change
```
kubectl get configmap aws-auth -o yaml -n kube-system
```
![Create STS Assume IAM Role for AWS CodeBuild](../images/5.createstsassumerole/5.7.createstsassumerole.png?pc=90pt)

2. Set ROLE value.
```
ROLE="    - rolearn: arn:aws:iam::$ACCOUNT_ID:role/EksCodeBuildKubectlRole\n      username: build\n      groups:\n        - system:masters"
```
![Create STS Assume IAM Role for AWS CodeBuild](../images/5.createstsassumerole/5.8.createstsassumerole.png?pc=90pt)

3. Get current aws-auth configMap data and attach new role info to it.
```
kubectl get -n kube-system configmap/aws-auth -o yaml | awk "/mapRoles: \|/{print;print \"$ROLE\";next}1" > /tmp/aws-auth-patch.yml
```
![Create STS Assume IAM Role for AWS CodeBuild](../images/5.createstsassumerole/5.9.createstsassumerole.png?pc=90pt)

4. Patch the aws-auth configmap with new role.
```
kubectl patch configmap/aws-auth -n kube-system --patch "$(cat /tmp/aws-auth-patch.yml)"
```
![Create STS Assume IAM Role for AWS CodeBuild](../images/5.createstsassumerole/5.10.createstsassumerole.png?pc=90pt)

5. Verify what is updated in aws-auth configmap after change
```
kubectl get configmap aws-auth -o yaml -n kube-system
```
![Create STS Assume IAM Role for AWS CodeBuild](../images/5.createstsassumerole/5.11.createstsassumerole.png?pc=90pt)