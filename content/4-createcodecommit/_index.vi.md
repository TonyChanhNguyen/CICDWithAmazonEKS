---
title : "Tạo CodeCommit Repository"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

1. Đi đến [CodeCommit](https://ap-southeast-1.console.aws.amazon.com/codesuite/codecommit/repositories?region=ap-southeast-1). Nhấn **Create repository**.
![Tạo CodeCommit Repository](../../../images/4.createcodecommit/4.1.createcodecommit.png?pc=90pt)

2. Tại mục **Repository name**, nhập ```fcj-cicd-source```.
3. Tại mục **Description**, nhập ```Codecommit repository to store source code for FCJ Workshop```.
4. Nhấn **Create**.
![Tạo CodeCommit Repository](../../../images/4.createcodecommit/4.2.createcodecommit.png?pc=90pt)

5. Nhấn **Clone URL** và chọn **Clone HTTPS** để lấy repository URL.
![Tạo CodeCommit Repository](../../../images/4.createcodecommit/4.3.createcodecommit.png?pc=90pt)

Repository URL được sao chép tự động.


6. Tại cửa sổ lệnh Cloud9, thực thi câu lệnh bên dưới để tải xuống git repository từ Code Commit vào local repository. Thay thế <Your-repository-URL> với repository URL đã sao chép.
```
git clone <Your-repository-URL>
```
![Tạo CodeCommit Repository](../../../images/4.createcodecommit/4.4.createcodecommit.png?pc=90pt)

Có một thư mục rỗng mới tên **fcj-cicd-source** xuất hiện.

7. Di chuyển tất cả tài nguyên đã tạo vào **fcj-cicd-source**.
```
mv app fcj-cicd-source
mv kube-manifest fcj-cicd-source
mv Dockerfile fcj-cicd-source
mv buildspec.yaml fcj-cicd-source
ls fcj-cicd-source
```
![Tạo CodeCommit Repository](../../../images/4.createcodecommit/4.5.createcodecommit.png?pc=90pt)

8. Đẩy code lên CodeCommit Repository.
```
cd fcj-cicd-source
git add .
git commit -m "Add all source codes"
git push
```
![Tạo CodeCommit Repository](../../../images/4.createcodecommit/4.6.createcodecommit.png?pc=90pt)

9. Tải lại trang CodeCommit Repository để xác minh có các tài nguyên đã được đẩy lên.
![Tạo CodeCommit Repository](../../../images/4.createcodecommit/4.7.createcodecommit.png?pc=90pt)

