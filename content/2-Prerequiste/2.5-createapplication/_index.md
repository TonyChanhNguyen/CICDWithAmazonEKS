---
title : "Create Application"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---
### Create application
1. At the Cloud9 terminal, enter the command below to create a new directory for application.
```
mkdir app
cd app
```
![Create application](../../images/2.prerequisites/2.5.createapp/2.5.1.createapp.png?pc=60pt)

2. Create a file named **index.html** inside **app**.
```
touch index.html
ls 
```
![Create application](../../images/2.prerequisites/2.5.createapp/2.5.2.createapp.png?pc=60pt)

3. Open file **index.html**, paste the below code and save it.
```
<!DOCTYPE html>
<html>
   <body style="background-color:rgb(228, 250, 210);">
      <h1>Welcome to First Cloud Journey - App Version - V1 </h1>
      <h3>Create CICD Pipeline to deploy application on Amazon EKS Cluster Workshop</h3>
      <p>Application Name: App1</p>
   </body>
</html>
```
![Create application](../../images/2.prerequisites/2.5.createapp/2.5.3.createapp.png?pc=60pt)

4. Create **Dockerfile**.
```
cd ..
touch Dockerfile
```
![Create application](../../images/2.prerequisites/2.5.createapp/2.5.4.createapp.png?pc=60pt)

5. Open **Dockerfile**, paste the below code and save it.
```
FROM nginx
COPY app /usr/share/nginx/html/app1
```
![Create application](../../images/2.prerequisites/2.5.createapp/2.5.5.createapp.png?pc=60pt)

### Create Manifest file
1. Create new directory to store Manifest files.
```
mkdir kube-manifest
ls
```
![Create Manifest file](../../images/2.prerequisites/2.5.createapp/2.5.6.createapp.png?pc=60pt)

2. Create new file **01-cicd-app-Deployment.yaml**.
```
touch kube-manifest/01-cicd-app-Deployment.yaml
```
![Create Manifest file](../../images/2.prerequisites/2.5.createapp/2.5.7.createapp.png?pc=60pt)

3. Open file **01-cicd-app-Deployment.yaml**, paste the below code and save it.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fcj-cicd-deployment
  labels:
    app: fcj-cicd
spec:
  replicas: 1
  selector:
    matchLabels:
      app: fcj-cicd
  template:
    metadata:
      labels:
        app: fcj-cicd
    spec:
      containers:
        - name: fcj-cicd
          image: CONTAINER_IMAGE
          ports:
            - containerPort: 80
```
![Create Manifest file](../../images/2.prerequisites/2.5.createapp/2.5.8.createapp.png?pc=60pt)

4. Create new file **02-cicd-app-NodePort-svc.yaml**.
```
touch kube-manifest/02-cicd-app-NodePort-svc.yaml
```
![Create Manifest file](../../images/2.prerequisites/2.5.createapp/2.5.9.createapp.png?pc=60pt)

5. Open file **02-cicd-app-NodePort-svc.yaml**, paste the below code and save it.
```
apiVersion: v1
kind: Service
metadata:
  name: fcj-cicd-nodeport-service
  labels:
    app: fcj-cicd
spec:
  type: NodePort
  selector:
    app: fcj-cicd
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30000
```
![Create Manifest file](../../images/2.prerequisites/2.5.createapp/2.5.10.createapp.png?pc=60pt)

### Create buildspec.yaml
1. Create a file named **buildspec.yaml**.
```
touch buildspec.yaml
```

2. Open file **buildspec.yaml**, paste the below code. At line 18, replace ```<REPLACE-WITH-YOUR-ACCOUNT-ID>``` with your Account ID then save it.
```
version: 0.2
phases:
  install:
    commands:
      - echo "Install Phase - Nothing to do using latest Amazon Linux Docker Image for CodeBuild which has all AWS Tools - https://github.com/aws/aws-codebuild-docker-images/blob/master/al2/x86_64/standard/3.0/Dockerfile"
  pre_build:
      commands:
        # Docker Image Tag with Date Time & Code Buiild Resolved Source Version
        - TAG="$(date +%Y-%m-%d.%H.%M.%S).$(echo $CODEBUILD_RESOLVED_SOURCE_VERSION | head -c 8)"
        # Update Image tag in our Kubernetes Deployment Manifest        
        - echo "Update Image tag in kube-manifest..."
        - sed -i 's@CONTAINER_IMAGE@'"$REPOSITORY_URI:$TAG"'@' kube-manifest/01-cicd-app-Deployment.yaml
        # Verify AWS CLI Version        
        - echo "Verify AWS CLI Version..."
        - aws --version
        # Login to ECR Registry for docker to push the image to ECR Repository
        - echo "Login in to Amazon ECR..."
        - aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <REPLACE-WITH-YOUR-ACCOUNT-ID>.dkr.ecr.ap-southeast-1.amazonaws.com
        # Update Kube config Home Directory
        - export KUBECONFIG=$HOME/.kube/config
  build:
    commands:
      # Build Docker Image
      - echo "Build started on `date`"
      - echo "Building the Docker image..."
      - docker build --tag $REPOSITORY_URI:$TAG .
  post_build:
    commands:
      # Push Docker Image to ECR Repository
      - echo "Build completed on `date`"
      - echo "Pushing the Docker image to ECR Repository"
      - docker push $REPOSITORY_URI:$TAG
      - echo "Docker Image Push to ECR Completed -  $REPOSITORY_URI:$TAG"    
      # Extracting AWS Credential Information using STS Assume Role for kubectl
      - echo "Setting Environment Variables related to AWS CLI for Kube Config Setup"          
      - CREDENTIALS=$(aws sts assume-role --role-arn $EKS_KUBECTL_ROLE_ARN --role-session-name codebuild-kubectl --duration-seconds 900)
      - export AWS_ACCESS_KEY_ID="$(echo ${CREDENTIALS} | jq -r '.Credentials.AccessKeyId')"
      - export AWS_SECRET_ACCESS_KEY="$(echo ${CREDENTIALS} | jq -r '.Credentials.SecretAccessKey')"
      - export AWS_SESSION_TOKEN="$(echo ${CREDENTIALS} | jq -r '.Credentials.SessionToken')"
      - export AWS_EXPIRATION=$(echo ${CREDENTIALS} | jq -r '.Credentials.Expiration')
      # Setup kubectl with our EKS Cluster              
      - echo "Update Kube Config"      
      - aws eks update-kubeconfig --name $EKS_CLUSTER_NAME
      # Apply changes to our Application using kubectl
      - echo "Apply changes to kube manifest"            
      - kubectl apply -f kube-manifest/
      - echo "Completed applying changes to Kubernetes Objects"           
      # Create Artifacts which we can use if we want to continue our pipeline for other stages
      - printf '[{"name":"01-cicd-app-Deployment.yaml","imageUri":"%s"}]' $REPOSITORY_URI:$TAG > build.json
      # Additional Commands to view your credentials      
      #- echo "Credentials Value is..  ${CREDENTIALS}"      
      #- echo "AWS_ACCESS_KEY_ID...  ${AWS_ACCESS_KEY_ID}"            
      #- echo "AWS_SECRET_ACCESS_KEY...  ${AWS_SECRET_ACCESS_KEY}"            
      #- echo "AWS_SESSION_TOKEN...  ${AWS_SESSION_TOKEN}"            
      #- echo "AWS_EXPIRATION...  $AWS_EXPIRATION"             
      #- echo "EKS_CLUSTER_NAME...  $EKS_CLUSTER_NAME"             
artifacts:
  files: 
    - build.json   
    - kube-manifests/*
```