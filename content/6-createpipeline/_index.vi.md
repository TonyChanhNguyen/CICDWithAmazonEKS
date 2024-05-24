---
title : "Tạo CodePipeline"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

1. Đi đến [CodePipeline](https://ap-southeast-1.console.aws.amazon.com/codesuite/codepipeline/pipelines?region=ap-southeast-1). Nhấn **Create pipeline**.

![Tạo CodePipeline](../../images/6.createpipeline/6.1.createpipeline.png?pc=90pt)

2. Tại mục **Pipeline name**, nhập tên của Pipeline ```FCJ-CICD-EKS-CodePipeline```.

![Tạo CodePipeline](../../images/6.createpipeline/6.2.createpipeline.png?pc=90pt)

3. Giữ mặc định ở các mục khác. Sau đó, cuộn xuống và nhấn **Next**.

![Tạo CodePipeline](../../images/6.createpipeline/6.3.createpipeline.png?pc=90pt)

4. Tại trang **Add source stage**, cấu hình thông số bên dưới:
+ Tại **Source provider**, chọn **AWS CodeCommit**.
+ Tại **Repository name**, chọn repository đã tạo tên **fcj-cicd-source**.
+ Tại **Branch name**, chọn nhánh **master**.
+ Tại **Change detection options**, chọn mặc định (**Amazon CloudWatch Events (recommended)**).
+ Tại **Output artifact format**, chọn mặc định (**CodePipeline default**).
5. Sau đó, nhấn **Next**.
![Tạo CodePipeline](../../images/6.createpipeline/6.4.createpipeline.png?pc=90pt)

6. Tại trang **Add build stage**, cấu hình thông số bên dưới:
+ Tại **Build provider**, chọn **AWS CodeBuild**.
+ Tại **Region**, chọn **Asian Pacific (Singapore)**.

7. Tại **Project name**, nhấn **Create project**.

![Tạo CodePipeline](../../images/6.createpipeline/6.5.createpipeline.png?pc=90pt)

8. Tại trang **Create build project**, nhập ```FCJ-CICD-EKS-CodeBuildPrj``` là **Project name** và giữ mặc định các mục còn lại.
![Tạo CodePipeline](../../images/6.createpipeline/6.6.createpipeline.png?pc=90pt)

9. Nhấn **Additional configuration** để mở rộng thêm cấu hình.
![Tạo CodePipeline](../../images/6.createpipeline/6.7.createpipeline.png?pc=90pt)

10. Thêm **Environment variables** bên dưới:
+ ```REPOSITORY_URI``` = ```<REPLACE-WITH-YOUR-ACCOUNT-ID>.dkr.ecr.ap-southeast-1.amazonaws.com/fcj-cicd-repository```
+ ```EKS_KUBECTL_ROLE_ARN``` = ```arn:aws:iam::<REPLACE-WITH-YOUR-ACCOUNT-ID>:role/EksCodeBuildKubectlRole```
+ ```EKS_CLUSTER_NAME``` = ```fcj-cicd-cluster```
![Tạo CodePipeline](../../images/6.createpipeline/6.8.createpipeline.png?pc=90pt)


11. Tại mục **Build specifications**, chọn **Use a buildspec file**.
![Tạo CodePipeline](../../images/6.createpipeline/6.9.createpipeline.png?pc=90pt)

12. Tại mục **Logs**, nhập ```FCJ-CICD-EKS-CodeBuildPrj``` tại **Group Name**.
13. Nhấn **Continue to CodePipeline**.
![Tạo CodePipeline](../../images/6.createpipeline/6.10.createpipeline.png?pc=89pt)

14. Bạn sẽ nhận được thông báo rằng CodeBuild project đã tạo thành công.

15. Sau đó, nhấn **Next**.

![Tạo CodePipeline](../../images/6.createpipeline/6.11.createpipeline.png?pc=90pt)

16. Tại trang **Add deploy stage**, nhấn **Skip deploy stage** để bỏ qua bước này.
17. Nhấn **Skip** để xác nhận.
![Tạo CodePipeline](../../images/6.createpipeline/6.12.createpipeline.png?pc=90pt)


18. Tại trang **Review**, hãy xem lại các cấu hình của bạn. Sau đó, nhấn **Create pipeline**.

![Tạo CodePipeline](../../images/6.createpipeline/6.13.createpipeline.png?pc=90pt)

