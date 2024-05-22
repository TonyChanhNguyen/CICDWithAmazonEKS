---
title : "Create STS Assume IAM Role for AWS CodeBuild"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

Trong một AWS CodePipeline, chúng ta sử dụng AWS CodeBuild để triển khai sự thay đổi lên các tệp Kubernetes Manifest của chúng ta.
Điều này cần một AWS IAM có khả năng tương tác với EKS Cluster.
Trong bước này, chúng ta sẽ tạo một IAM Role và thêm quy tắc EKS:Describe mà chúng ta sẽ sử dụng trong CodeBuild để thao tác với EKS Cluster thông qua kubectl.



### Tạo IAM Role
1. Tại cửa sổ lệnh Cloud9, xuất ra Account ID.
```
export ACCOUNT_ID=<REPLACE-WITH-YOUR-ACCOUNT-ID>
```
![Tạo STS Assume IAM Role cho AWS CodeBuild](../../../images/5.createstsassumerole/5.1.createstsassumerole.png?pc=90pt)

2. Đặt Trust Policy.
```
TRUST="{ \"Version\": \"2012-10-17\", \"Statement\": [ { \"Effect\": \"Allow\", \"Principal\": { \"AWS\": \"arn:aws:iam::${ACCOUNT_ID}:root\" }, \"Action\": \"sts:AssumeRole\" } ] }"
echo $TRUST
```
![Tạo STS Assume IAM Role cho AWS CodeBuild](../../../images/5.createstsassumerole/5.2.createstsassumerole.png?pc=90pt)

3. Tạo IAM Role cho CodeBuild để tương tác với EKS.
```
aws iam create-role --role-name EksCodeBuildKubectlRole --assume-role-policy-document "$TRUST" --output text --query 'Role.Arn'
```
![Tạo STS Assume IAM Role cho AWS CodeBuild](../../../images/5.createstsassumerole/5.3.createstsassumerole.png?pc=90pt)

4. Định nghĩa Inline Policy với quyền eks Describe trong tệp iam-eks-describe-policy.
```
echo '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": "eks:Describe*", "Resource": "*" } ] }' > /tmp/iam-eks-describe-policy
```
![Tạo STS Assume IAM Role cho AWS CodeBuild](../../../images/5.createstsassumerole/5.4.createstsassumerole.png?pc=90pt)

5. Liên kết Inline Policy với IAM Role mới vừa tạo.
```
aws iam put-role-policy --role-name EksCodeBuildKubectlRole --policy-name eks-describe --policy-document file:///tmp/iam-eks-describe-policy
```
![Tạo STS Assume IAM Role cho AWS CodeBuild](../../../images/5.createstsassumerole/5.5.createstsassumerole.png?pc=90pt)

6. Đi đến [IAM Role](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/roles) để xác minh có một IAM Role được tạo tên ```EksCodeBuildKubectlRole```.
![Tạo STS Assume IAM Role cho AWS CodeBuild](../../../images/5.createstsassumerole/5.6.createstsassumerole.png?pc=90pt)


### Cập nhật EKS Cluster aws-auth ConfigMap với Role vừa tạo.
1. Xác minh aws-auth configmap trước thay đổi.
```
kubectl get configmap aws-auth -o yaml -n kube-system
```
![Tạo STS Assume IAM Role cho AWS CodeBuild](../../../images/5.createstsassumerole/5.7.createstsassumerole.png?pc=90pt)

2. Đặt giá trị ROLE.
```
ROLE="    - rolearn: arn:aws:iam::$ACCOUNT_ID:role/EksCodeBuildKubectlRole\n      username: build\n      groups:\n        - system:masters"
```
![Tạo STS Assume IAM Role cho AWS CodeBuild](../../../images/5.createstsassumerole/5.8.createstsassumerole.png?pc=90pt)

3. Lấy dữ liệu aws-auth configMap hiện tại và gán Role mới vào nó.
```
kubectl get -n kube-system configmap/aws-auth -o yaml | awk "/mapRoles: \|/{print;print \"$ROLE\";next}1" > /tmp/aws-auth-patch.yml
```
![Tạo STS Assume IAM Role cho AWS CodeBuild](../../../images/5.createstsassumerole/5.9.createstsassumerole.png?pc=90pt)

4. Cập nhật aws-auth configmap với Role mới.
```
kubectl patch configmap/aws-auth -n kube-system --patch "$(cat /tmp/aws-auth-patch.yml)"
```
![Tạo STS Assume IAM Role cho AWS CodeBuild](../../../images/5.createstsassumerole/5.10.createstsassumerole.png?pc=90pt)

5. Xác minh in aws-auth configmap sau khi cập nhật.
```
kubectl get configmap aws-auth -o yaml -n kube-system
```
![Tạo STS Assume IAM Role cho AWS CodeBuild](../../../images/5.createstsassumerole/5.11.createstsassumerole.png?pc=90pt)