---
title : "Kiểm tra kết quả"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---


Bạn vừa tạo xong một Pipeline để triển khai ứng dụng lên Amazon EKS ở bước trước. Hãy xác minh Pipeline của bạn và tạo thay đổi trên code để kiểm tra kết quả.

### Xác minh Pipeline.
1. Đi đến [AWS CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1), bạn sẽ thấy có một Pipeline tên **FCJ-CICD-EKS-CodePipeline** nhưng **Latest execution status** là **failed**. Nhấn vào Pipeline để điều tra nguyên nhân.

![Kiểm tra kết quả](../../../images/7.verifyresult/7.1.verifyresult.png?pc=90pt)

2. Lỗi xảy ra ở bước **Build**, nhấn **View details** để thêm thông tin.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.2.verifyresult.png?pc=90pt)

3. Cuộn xuống cuối và thấy lỗi bởi EKS Cluster không thể đăng nhập vào ECR, bởi vì *no identity-based policy allows the ecr:GetAuthorizationToken action* trong IAM Role **codebuild-FCJ-CICD-EKS-CodeBuildPrj-service-role**. Nghĩa là CodeBuild không có khả năng làm mới và đẩy Docker Image tạo mới lên ECR repository.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.3.verifyresult.png?pc=90pt)

4. Bây giờ chúng ta cần gán quy tắc mới **ecr:GetAuthorizationToken** vào IAM Role. Đi đến [IAM Policies](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/policies) để tìm kiếm quy tắc tên ```AmazonEC2ContainerRegistryPowerUser```, nó có hành động **ecr:GetAuthorizationToken** vì thế chúng ta có thể sử dụng nó để gán vào IAM Role.

![Kiểm tra kết quả](../../../images/7.verifyresult/7.4.verifyresult.png?pc=90pt)

5. Đi đến [IAM Roles](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/roles), tìm kiếm IAM Role ```codebuild-FCJ-CICD-EKS-CodeBuildPrj-service-role```. Nhấn vào nó.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.5.verifyresult.png?pc=90pt)

6. Gán quy tắc tên ```AmazonEC2ContainerRegistryPowerUser``` vào IAM Role.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.6.verifyresult.png?pc=90pt)

7. Sau khi gán quy tắc vào IAM Role thành công, trở lại [AWS CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines/FCJ-CICD-EKS-CodePipeline/view?region=ap-southeast-1). Nhấn **Retry failed actions**.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.7.verifyresult.png?pc=90pt)

![Kiểm tra kết quả](../../../images/7.verifyresult/7.8.verifyresult.png?pc=90pt)


8. Sau khi bước **Build** hoàn tất, kết quả trả về vẫn lỗi. Nhấn vào **View details** để thấy nguyên nhân.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.9.verifyresult.png?pc=90pt)

9. Nguyên nhân là CodeBuild không có truy cập để thực hiện quyền cập nhật EKS Cluster. Hãy tạo STS Assume Policy và liên kết với CodeBuild Role ```codebuild-FCJ-CICD-EKS-CodeBuildPrj-service-role```.

![Kiểm tra kết quả](../../../images/7.verifyresult/7.10.verifyresult.png?pc=90pt)

10. Đi đến [IAM Policies](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/policies). Nhấn **Create policy**.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.11.verifyresult.png?pc=90pt)

11. Tại trang **Specify permissions**, cấu hình thông số bên dưới:
+ Tại mục **Service**, chọn **STS**.
+ Tại mục **Actions allowed**, chọn **AssumeRole** là hành động **write**.
+ Tại mục **Resources**, thêm ARN của bạn.

12. Sau đó nhấn **Next**.

![Kiểm tra kết quả](../../../images/7.verifyresult/7.12.verifyresult.png?pc=90pt)

13. Tại trang **Review and create**, nhập các giá trị:
+ Nhập ```fcj-codebuild-sts-assume-role``` là **Policy name** .
+ Nhập ```CodeBuild to interact with EKS cluster to perform changes``` là **Description - optional**.

14. Sau đó nhấn **Create policy**.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.13.verifyresult.png?pc=90pt)

15. Đi đến [IAM Roles](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/roles/details/EksCodeBuildKubectlRole?section=permissions) và tìm kiếm IAM Role tên ```codebuild-FCJ-CICD-EKS-CodeBuildPrj-service-role```.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.14.verifyresult.png?pc=90pt)

16. Gán quy tắc đã tạo ở trên vào IAM Role.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.15.verifyresult.png?pc=90pt)

17. Trở lại Pipeline của bạn và nhấn **Retry stage** để chạy lại **Build Step**.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.16.verifyresult.png?pc=90pt)

18. Sau vài phút, Pipeline của bạn đã được xây dựng thành công.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.17.verifyresult.png?pc=90pt)

### Kiểm tra kết quả.
1. Tại cửa sổ lệnh Cloud9, liệt kê tất cả Deployment, Service và Pod đã tạo trong EKS Cluster.
```
kubectl get deploy,svc,pod -o wide
```
![Kiểm tra kết quả](../../../images/7.verifyresult/7.18.verifyresult.png?pc=90pt)

Các tài nguyên đã được tạo:
+ Một Deployment tên **fcj-cicd-Deployment** với **Replicas** là **1** và **IMAGES** là `<YOUR-ACCOUNT-ID>.dkr.ecr.ap-southeast-1.amazonaws.com/fcj-cicd-repository:<TAG-NAME>` (In my case **IMAGES** is **170074558790.dkr.ecr.ap-southeast-1.amazonaws.com/fcj-cicd-repository:2024-05-21.20.59.18.1303bea5**).
+ Một Service tên **fcj-cicd-nodeport-service** với **Type** là **NodePort** và **PORT** là **80:30000/TCP**.
+ Một Pod tên **fcj-cicd-deployment-xxxxxxxxxx-xxxxx** với **STATUS** là **Running**.

2. Đi đến [ECR](https://ap-southeast-1.console.aws.amazon.com/ecr/private-registry/repositories?region=ap-southeast-1), nhấn vào repository của bạn.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.19.verifyresult.png?pc=90pt)

3. Có một Image được tạo trên Repository với **Image tag** khớp với `<TAG-NAME>` trong **IMAGES** của **fcj-cicd-Deployment**.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.20.verifyresult.png?pc=90pt)


4. Lấy **EXTERNAL-IP** của Node.
```
kubectl get node -o wide
```
![Kiểm tra kết quả](../../../images/7.verifyresult/7.21.verifyresult.png?pc=90pt)

5. Truy cập đường dẫn ứng dụng ```http://<NODE's-EXTERNAL-IP>:30000```.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.22.verifyresult.png?pc=90pt)

Bạn không thể truy cập vào ứng dụng vì cổng 30000 của Node vẫn chưa được mở.

6. Đi đến Security Group của Node.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.23.verifyresult.png?pc=90pt)

7. Nhấn **Edit inbound rules**.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.24.verifyresult.png?pc=90pt)

8. Thêm quy tắc mới:
+ **Type** là **Custom TCP**.
+ **Port range** là **30000**.
+ **Source** là **Anywhere - IPv4**.
9. Sau đó, nhấn **Save rules**.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.25.verifyresult.png?pc=90pt)

10. Hãy tải lại đường dẫn ứng dụng để kiểm tra kết quả.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.26.verifyresult.png?pc=90pt)

Ứng dụng của bạn đã hoạt động với phiên bản V1.

### Tạo sự thay đổi trong index.html.
1. Trở lại máy chủ Cloud9, mở tệp **index.html** bên trong **fcj-cicd-source/app**. Thay đổi code thành phiên bản **V2** và lưu lại.

![Kiểm tra kết quả](../../../images/7.verifyresult/7.27.verifyresult.png?pc=90pt)

2. Đẩy code lên **AWS CodeCommit**.
```
git add .
git commit -m "Modify to version V2"
git push
```
![Kiểm tra kết quả](../../../images/7.verifyresult/7.28.verifyresult.png?pc=90pt)

3. Đi đến [AWS CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines/FCJ-CICD-EKS-CodePipeline/view?region=ap-southeast-1) để xác minh Pipeline đã được kích hoạt để chạy các quy trình.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.29.verifyresult.png?pc=90pt)

4. Chờ khi Pipeline hoàn tất với trạng thái là thành công. Tải lại đường dẫn ứng dụng để kiểm tra kết quả.
![Kiểm tra kết quả](../../../images/7.verifyresult/7.30.verifyresult.png?pc=90pt)
![Kiểm tra kết quả](../../../images/7.verifyresult/7.31.verifyresult.png?pc=90pt)

Ứng dụng của bạn đã hoạt động với phiên bản V2.

#### Chúc mừng, bạn đã triển khi phiên bản mới lên Amazon EKS Cluster với AWS CodePipeline thành công!

