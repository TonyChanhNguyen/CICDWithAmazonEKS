---
title : "Create CodeCommit Repository"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

1. Go to [CodeCommit](https://ap-southeast-1.console.aws.amazon.com/codesuite/codecommit/repositories?region=ap-southeast-1). Click on **Create repository**.
![Create CodeCommit Repository](../../images/4.createcodecommit/4.1.createcodecommit.png?pc=90pt)

2. At **Repository name** field, input ```fcj-cicd-source```.
3. At **Description** field, input ```Codecommit repository to store source code for FCJ Workshop```.
4. Click on **Create**.
![Create CodeCommit Repository](../../images/4.createcodecommit/4.2.createcodecommit.png?pc=90pt)

5. Click to **Clone URL** and select **Clone HTTPS** to get your repository URL.
![Create CodeCommit Repository](../../images/4.createcodecommit/4.3.createcodecommit.png?pc=90pt)

The repository URL is copied automatically.


6. At Cloud9 Terminal, execute the below commands to clone the git repository from Code Commit to local repository. Replace <Your-repository-URL> with your copied repository URL.
```
git clone <Your-repository-URL>
```
![Create CodeCommit Repository](../../images/4.createcodecommit/4.4.createcodecommit.png?pc=90pt)

There is a new empty folder named **fcj-cicd-source** appeared in your workspace.

7. Move all your created resources to **fcj-cicd-source**.
```
mv app fcj-cicd-source
mv kube-manifest fcj-cicd-source
mv Dockerfile fcj-cicd-source
mv buildspec.yaml fcj-cicd-source
ls fcj-cicd-source
```
![Create CodeCommit Repository](../../images/4.createcodecommit/4.5.createcodecommit.png?pc=90pt)

8. Push your source code to CodeCommit Repository.
```
cd fcj-cicd-source
git add .
git commit -m "Add all source codes"
git push
```
![Create CodeCommit Repository](../../images/4.createcodecommit/4.6.createcodecommit.png?pc=90pt)

9. Reload your CodeCommit Repository to verify there are some resources pushed to.
![Create CodeCommit Repository](../../images/4.createcodecommit/4.7.createcodecommit.png?pc=90pt)

