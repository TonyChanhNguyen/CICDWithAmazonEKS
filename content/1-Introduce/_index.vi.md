---
title : "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

### Tổng quan

Quy trình CI/CD rất cần thiết để tự động hóa việc triển khai ứng dụng. Bài thực hành này liệt kê các bước cần thiết để thiết lập một quy trình CI/CD để triển khai ứng dụng lên một Amazon Elastic Kubernetes Service (EKS) cluster.


![Create CI/CD Pipeline to deploy application to Amazon EKS](../../images/ekscicd.png?pc=60pt)

+ [AWS CodePipeline](https://aws.amazon.com/codepipeline/?nc1=h_ls):là dịch vụ phân phối liên tục được quản lý hoàn toàn giúp bạn tự động hóa các quy trình phát hành để cung cấp các bản cập nhật về ứng dụng và cơ sở hạ tầng một cách nhanh chóng và ổn định.
+ [AWS CodeCommit](https://aws.amazon.com/codecommit/?nc1=h_ls):  là một dịch vụ kiểm soát nguồn được quản lý hoàn toàn, an toàn và có quy mô linh hoạt cao để lưu trữ các kho Git riêng tư.
+ [AWS CodeBuild](https://aws.amazon.com/codebuild/): là dịch vụ tích hợp liên tục được quản lý hoàn toàn giúp biên dịch mã nguồn, chạy kiểm thử và sản xuất các gói phần mềm sẵn sàng triển khai.
+ [AWS ECR](https://aws.amazon.com/ecr/?nc1=h_ls): là sổ đăng ký bộ chứa được quản lý hoàn toàn, cung cấp dịch vụ lưu trữ hiệu suất cao để bạn có thể triển khai thành phần lạ và hình ảnh ứng dụng ở bất kỳ đâu một cách đáng tin cậy.
+ [Amazon S3](https://aws.amazon.com/s3/?nc1=h_ls): là một dịch vụ lưu trữ đối tượng cung cấp khả năng thay đổi quy mô, mức độ sẵn sàng của dữ liệu, độ bảo mật và hiệu suất hàng đầu trong ngành. Khách hàng thuộc mọi quy mô và ngành nghề có thể lưu trữ và bảo vệ dữ liệu thuộc mọi kích thước cho hầu hết tất cả các trường hợp sử dụng, chẳng hạn như hồ dữ liệu, ứng dụng hoạt động trên đám mây và ứng dụng di động. Với các lớp lưu trữ tiết kiệm chi phí và tính năng quản lý dễ sử dụng, bạn có thể tối ưu hóa chi phí, tổ chức dữ liệu và cấu hình các biện pháp kiểm soát quyền truy cập được tinh chỉnh để đáp ứng yêu cầu cụ thể về kinh doanh, tổ chức và tuân thủ.

### Nội dung

1. [Giới thiệu](../../1-introduce/)
2. [Các bước chuẩn bị](../../2-prerequiste/)
3. [Tạo Amazon ECR](../../3-interactwithecr/)
4. [Tạo AWS CodeCommit](../../4-createcodecommit/)
5. [Tạo STS Assume IAM Role cho AWS CodeBuild](../../5-createstsiamrole/)
6. [Tạo AWS CodePipeline](../../6-createpipeline/)
7. [Kiểm tra kết quả](../../7-verifyresult/)
8. [Dọn dẹp tài nguyên](../../8-cleanup/)
