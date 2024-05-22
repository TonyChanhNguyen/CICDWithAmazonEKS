---
title : "Tạo quy trình CI/CD để triển khai ứng dụng lên Amazon EKS Cluster"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---
# Tạo quy trình CI/CD để triển khai ứng dụng lên Amazon EKS Cluster

### Tổng quan

Quy trình CI/CD rất cần thiết để tự động hóa việc triển khai ứng dụng. Bài thực hành này liệt kê các bước cần thiết để thiết lập một quy trình CI/CD để triển khai ứng dụng lên một Amazon Elastic Kubernetes Service (EKS) cluster.

![Tạo quy trình CI/CD để triển khai ứng dụng lên Amazon EKS Cluster](../images/ekscicd.png?pc=60pt)

### Nội dung

1. [Giới thiệu](../1-introduce/)
2. [Các bước chuẩn bị](../2-prerequiste/)
3. [Tạo Amazon ECR](../3-interactwithecr/)
4. [Tạo AWS CodeCommit](../4-createcodecommit/)
5. [Tạo STS Assume IAM Role cho AWS CodeBuild](../5-createstsiamrole/)
6. [Tạo AWS CodePipeline](../6-createpipeline/)
7. [Kiểm tra kết quả](../7-verifyresult/)
8. [Dọn dẹp tài nguyên](../8-cleanup/)