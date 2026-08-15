# 🛍️ TrendStore – DevOps Project

A complete DevOps and Cloud deployment project demonstrating source-code management, Docker containerization, Docker Hub image management, Amazon EKS orchestration, Kubernetes deployment, AWS Application Load Balancer (ALB) integration, Jenkins CI/CD automation, AWS infrastructure provisioning using Terraform, and monitoring.

---

## 📌 Project Overview

**TrendStore** is a web-based application deployed on AWS using a modern DevOps workflow.

The project implements:

* GitHub for source-code management
* Git for version control
* Docker for application containerization
* Docker Hub for Docker image management
* Terraform for AWS infrastructure provisioning
* Jenkins for CI/CD automation
* Amazon EKS for Kubernetes orchestration
* Kubernetes Deployment and Service
* AWS Load Balancer Controller
* Kubernetes Ingress
* AWS Application Load Balancer (ALB)
* Prometheus and Grafana for monitoring and logs

### Deployment Architecture

```text
Developer
   │
   ▼
GitHub Repository
   │
   │ Webhook
   ▼
Jenkins
   │
   ├── Source Code Checkout
   │
   ├── Docker Build
   │
   ├── Docker Image Tag
   │
   ├── Docker Image Push
   │
   └── Kubernetes Deployment
   │
   ▼
Docker Registry
   │
   │ Docker Image
   ▼
Amazon EKS
   │
   ├── Deployment
   │       │
   │       └── TrendStore Pods
   │
   ├── ClusterIP Service
   │
   └── Ingress
   │
   ▼
AWS Load Balancer Controller
   │
   ▼
Application Load Balancer (ALB)
   │
   ▼
Internet
   │
   ▼
TrendStore Application
```

---

# 🛠️ Technologies Used

| **Category**           | **Technology**                |
| ---------------------- | ----------------------------- |
| Source Code            | GitHub                        |
| Version Control        | Git                           |
| Application            | TrendStore                    |
| Containerization       | Docker                        |
| Container Registry     | Docker Hub                    |
| Infrastructure as Code | Terraform                     |
| CI/CD Automation       | Jenkins                       |
| Orchestration          | Kubernetes                    |
| Kubernetes Platform    | Amazon EKS                    |
| Load Balancer          | AWS Application Load Balancer |
| Controller             | AWS Load Balancer Controller  |
| Ingress                | Kubernetes Ingress            |
| Monitoring             | Prometheus and Grafana        |
| Infrastructure         | AWS                           |
| Operating System       | Ubuntu / Amazon Linux         |
| Region                 | `ap-south-1`                  |

---

# 📁 Project Structure

```text
TrendStore/
│
├── Dockerfile
├── Jenkinsfile
├── k8s/
    ├── namespace.yaml
    ├── deployment.yaml
    ├── service.yaml
    ├── ingress.yaml
├── README.md
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── providers.tf
│
└── application files
```

---

# ☁️ AWS Environment

### AWS Region

```text
ap-south-1
```

### EKS Cluster

```text
trend-cluster
```

### Kubernetes Namespace

```text
trend
```

### Kubernetes Application

```text
trend-app
```

### Kubernetes Service

```text
trend-service
```

### Kubernetes Ingress

```text
trend-ingress
```

### AWS Account

The AWS account ID should not be hard-coded in public documentation or source files.

Use:

```text
AWS Account: <AWS-ACCOUNT-ID>
```

---

# 1. 🔧 GitHub Repository

The TrendStore project source code is maintained in GitHub.

Clone the repository:

```bash
git clone <GITHUB-REPOSITORY-URL>
```

Navigate to the project:

```bash
cd TrendStore
```

Check repository status:

```bash
git status
```

Add files:

```bash
git add .
```

Commit:

```bash
git commit -m "Update TrendStore DevOps project"
```

Push:

```bash
git push origin main
```

---

# 2. 🐳 Docker Containerization

TrendStore is containerized using Docker.

## Dockerfile

Example:

```dockerfile
FROM nginx:alpine

COPY dist/ /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

The Docker image contains the TrendStore static application files and Nginx web server.

---

## Build Docker Image

```bash
docker build -t trend-app:latest .
```

Verify:

```bash
docker images
```

Expected:

```text
REPOSITORY    TAG       IMAGE ID       CREATED
trend-app    latest    xxxxxxxxxxxx   ...
```

---

## Run Container Locally

```bash
docker run -d \
  --name trend-app \
  -p 8080:80 \
  trend-app:latest
```

Check the container:

```bash
docker ps
```

Test:

```text
http://localhost:8080
```

Stop the container:

```bash
docker stop trend-app
```

Remove the container:

```bash
docker rm trend-app
```

---

# 3. 🐳 Docker Hub

Docker Hub is used as the container registry to store and distribute the TrendStore Docker image.

The Docker image is built locally or by Jenkins, tagged with the Docker Hub repository name, and pushed to Docker Hub. Amazon EKS then pulls the image from Docker Hub during Kubernetes deployment.

---

## Create Docker Hub Repository

Log in to Docker Hub and create a new repository:

```text
Repository Name: trend-app
Visibility: Public
```

Example Docker Hub image:

```text
<DOCKERHUB-USERNAME>/trend-app:latest
```

---

## Authenticate Docker with Docker Hub

Login to Docker Hub:

```bash
docker login
```

Enter your Docker Hub credentials when prompted.

For automated Jenkins pipelines, configure Docker Hub credentials in Jenkins instead of storing the username or password directly in the Jenkinsfile.

---

## Build Docker Image

Build the TrendStore Docker image:

```bash
docker build -t trend-app:latest .
```

Verify:

```bash
docker images
```

Expected:

```text
REPOSITORY   TAG       IMAGE ID
trend-app    latest    xxxxxxxxxxxx
```

---

## Tag Docker Image

Tag the local Docker image with the Docker Hub repository:

```bash
docker tag trend-app:latest \
<DOCKERHUB-USERNAME>/trend-app:latest
```

Verify:

```bash
docker images
```

Expected:

```text
REPOSITORY                         TAG
trend-app                          latest
<DOCKERHUB-USERNAME>/trend-app    latest
```

---

## Push Image to Docker Hub

Push the image to Docker Hub:

```bash
docker push \
<DOCKERHUB-USERNAME>/trend-app:latest
```

Example:

```bash
docker push \
anushperamaiyang/trend-app:latest
```

The image will then be available in the Docker Hub repository:

```text
<DOCKERHUB-USERNAME>/trend-app
```

---

## Verify Docker Hub Image

Verify the image by checking the Docker Hub repository.

You can also verify locally:

```bash
docker images
```

Pull the image from Docker Hub to verify that it is publicly available:

```bash
docker pull \
<DOCKERHUB-USERNAME>/trend-app:latest
```

Run the image:

```bash
docker run -d \
  --name trend-app-test \
  -p 8080:80 \
  <DOCKERHUB-USERNAME>/trend-app:latest
```

Check:

```bash
docker ps
```

Test the application:

```text
http://localhost:8080
```

Remove the test container after verification:

```bash
docker stop trend-app-test
docker rm trend-app-test
```

---

## Docker Hub Image Used by Kubernetes

The Kubernetes Deployment uses the Docker Hub image:

```yaml
containers:
  - name: trend-app
    image: anushperamaiyang/trend-app:latest
    ports:
      - containerPort: 80
```

Amazon EKS will pull the Docker image from Docker Hub when creating the TrendStore Pods.

---

## Docker Hub Workflow

```text
TrendStore Source Code
        │
        ▼
     Docker Build
        │
        ▼
   trend-app:latest
        │
        ▼
    Docker Tag
        │
        ▼
Docker Hub Repository
        │
        │
        ▼
   Amazon EKS
        │
        ▼
 TrendStore Pods
```

### Docker Hub Repository

```text
Docker Hub Repository:
<DOCKERHUB-USERNAME>/trend-app
```

### Docker Image

```text
<DOCKERHUB-USERNAME>/trend-app:latest
```

This Docker Hub image is used by the Jenkins CI/CD pipeline and Amazon EKS Kubernetes deployment.


# 4. 🏗️ Terraform Infrastructure

Terraform is used to define and provision AWS infrastructure using Infrastructure as Code.

Typical infrastructure components include:

* VPC
* Public subnets
* Private subnets
* Internet Gateway
* NAT Gateway
* Route tables
* Security groups
* IAM roles
* EC2 infrastructure
* Jenkins server
* EKS-related infrastructure

Initialize Terraform:

```bash
terraform init
```

Validate configuration:

```bash
terraform validate
```

Format Terraform files:

```bash
terraform fmt
```

Review the execution plan:

```bash
terraform plan
```

Apply infrastructure:

```bash
terraform apply
```

Verify:

```bash
terraform output
```

Terraform provides a repeatable and version-controlled approach to AWS infrastructure provisioning.

---

# 5. ☸️ Amazon EKS

TrendStore is deployed to Amazon EKS.

## Verify AWS CLI

```bash
aws --version
```

## Verify EKS Cluster

```bash
aws eks describe-cluster \
  --name trend-cluster \
  --region ap-south-1
```

---

## Configure kubectl

```bash
aws eks update-kubeconfig \
  --region ap-south-1 \
  --name trend-cluster
```

Verify:

```bash
kubectl get nodes
```

Expected:

```text
NAME                                      STATUS   ROLES
ip-xxx.ap-south-1.compute.internal        Ready    <none>
ip-xxx.ap-south-1.compute.internal        Ready    <none>
```

---

# 6. 🚀 Kubernetes Deployment

The TrendStore application is deployed using a Kubernetes Deployment.

## deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: trend-app

spec:
  replicas: 2

  selector:
    matchLabels:
      app: trend-app

  template:
    metadata:
      labels:
        app: trend-app

    spec:
      containers:
        - name: trend-app

          image: <AWS-ACCOUNT-ID>.dkr.ecr.ap-south-1.amazonaws.com/trend-app:latest

          ports:
            - containerPort: 80
```

Apply:

```bash
kubectl apply -f deployment.yaml
```

Check deployment:

```bash
kubectl get deployments
```

Check pods:

```bash
kubectl get pods
```

Expected:

```text
trend-app-xxxxxxxxxx   1/1   Running
trend-app-yyyyyyyyyy   1/1   Running
```

---

# 7. 🔗 Kubernetes Service

Because the project uses an AWS Application Load Balancer through Kubernetes Ingress, the Service is configured as `ClusterIP`.

## service.yaml

```yaml
apiVersion: v1
kind: Service

metadata:
  name: trend-service

spec:
  selector:
    app: trend-app

  type: ClusterIP

  ports:
    - port: 80
      targetPort: 80
```

Apply:

```bash
kubectl apply -f service.yaml
```

Verify:

```bash
kubectl get svc
```

Expected:

```text
NAME           TYPE        CLUSTER-IP
trend-service  ClusterIP   10.x.x.x
```

---

# 8. ⚖️ AWS Application Load Balancer

TrendStore uses an AWS Application Load Balancer instead of a Kubernetes `LoadBalancer` Service.

Architecture:

```text
Internet
   │
   ▼
AWS Application Load Balancer
   │
   ▼
Kubernetes Ingress
   │
   ▼
ClusterIP Service
   │
   ▼
TrendStore Pods
```

The ALB provides external HTTP access to the application.

---

# 9. AWS Load Balancer Controller

The AWS Load Balancer Controller is responsible for provisioning and managing the AWS Application Load Balancer.

Verify:

```bash
kubectl get deployment \
  -n kube-system \
  aws-load-balancer-controller
```

Expected:

```text
NAME                           READY
aws-load-balancer-controller   2/2
```

Check pods:

```bash
kubectl get pods -n kube-system | grep aws-load-balancer
```

Expected:

```text
aws-load-balancer-controller-xxxxx   1/1   Running
aws-load-balancer-controller-yyyyy   1/1   Running
```

Check controller logs:

```bash
kubectl logs \
  -n kube-system \
  deployment/aws-load-balancer-controller
```

The controller watches Kubernetes Ingress resources and provisions the AWS Application Load Balancer.

---

# 10. 🌐 Kubernetes Ingress

## ingress.yaml

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: trend-ingress

  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip

spec:
  ingressClassName: alb

  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix

            backend:
              service:
                name: trend-service
                port:
                  number: 80
```

Apply:

```bash
kubectl apply -f ingress.yaml
```

Check:

```bash
kubectl get ingress
```

Expected:

```text
NAME           CLASS   HOSTS   ADDRESS
trend-ingress  alb     *       k8s-default-trend-xxxxx.ap-south-1.elb.amazonaws.com
```

---

# 11. 🔍 Verify Application Endpoints

Check Service endpoints:

```bash
kubectl get endpoints trend-service
```

Expected:

```text
NAME           ENDPOINTS
trend-service  192.168.x.x:80,192.168.x.x:80
```

If endpoints are empty, check Pod labels:

```bash
kubectl get pods --show-labels
```

The Pods must contain:

```text
app=trend-app
```

because the Service selector is:

```yaml
selector:
  app: trend-app
```

---

# 12. 🔗 Get ALB DNS Name

Run:

```bash
kubectl get ingress trend-ingress
```

Or:

```bash
kubectl get ingress trend-ingress \
  -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'
```

Example:

```text
k8s-default-trend-xxxxx.ap-south-1.elb.amazonaws.com
```

Access the application:

```text
http://<ALB-DNS-NAME>
```

---

# 13. 🆔 Get Application Load Balancer ARN

Retrieve the ALB information:

```bash
aws elbv2 describe-load-balancers \
  --region ap-south-1 \
  --query 'LoadBalancers[*].[LoadBalancerName,DNSName,LoadBalancerArn]' \
  --output table
```

Retrieve only the ARN:

```bash
aws elbv2 describe-load-balancers \
  --region ap-south-1 \
  --query 'LoadBalancers[*].LoadBalancerArn' \
  --output text
```

Example:

```text
arn:aws:elasticloadbalancing:ap-south-1:<AWS-ACCOUNT-ID>:loadbalancer/app/k8s-default-trend-xxxxx/xxxxxxxx
```

---

# 14. 🔧 Jenkins CI/CD

Jenkins is used to automate the TrendStore CI/CD process.

The Jenkins pipeline performs:

1. GitHub source-code checkout
2. Docker image build
3. Docker image tagging
4. Docker registry authentication
5. Docker image push
6. EKS authentication
7. Kubernetes deployment
8. Deployment verification

Architecture:

```text
GitHub
   │
   ▼
Jenkins
   │
   ├── Git Checkout
   │
   ├── Docker Build
   │
   ├── Docker Tag
   │
   ├── Docker Push
   │
   ├── EKS Authentication
   │
   └── kubectl apply
          │
          ▼
        EKS
```

---

# 15. 📄 Jenkinsfile

Example Jenkins pipeline:

```groovy
pipeline {

    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        ECR_REPOSITORY = 'trend-app'
        EKS_CLUSTER = 'trend-cluster'
        IMAGE_TAG = 'latest'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    docker build \
                      -t ${ECR_REPOSITORY}:${IMAGE_TAG} .
                '''
            }
        }

        stage('ECR Login') {
            steps {
                sh '''
                    AWS_ACCOUNT_ID=$(aws sts get-caller-identity \
                      --query Account \
                      --output text)

                    aws ecr get-login-password \
                      --region ${AWS_REGION} | \
                    docker login \
                      --username AWS \
                      --password-stdin \
                      ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                '''
            }
        }

        stage('Docker Tag and Push') {
            steps {
                sh '''
                    AWS_ACCOUNT_ID=$(aws sts get-caller-identity \
                      --query Account \
                      --output text)

                    ECR_URI=${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}

                    docker tag \
                      ${ECR_REPOSITORY}:${IMAGE_TAG} \
                      ${ECR_URI}:${IMAGE_TAG}

                    docker push \
                      ${ECR_URI}:${IMAGE_TAG}
                '''
            }
        }

        stage('Configure EKS') {
            steps {
                sh '''
                    aws eks update-kubeconfig \
                      --region ${AWS_REGION} \
                      --name ${EKS_CLUSTER}
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                    kubectl apply -f ingress.yaml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    kubectl rollout status deployment/trend-app
                    kubectl get pods
                    kubectl get svc
                    kubectl get ingress
                '''
            }
        }
    }
}
```

> Adjust the Jenkinsfile according to the Jenkins credentials, IAM role, Docker registry, ECR repository, and Kubernetes configuration used in the actual environment.

---

# 16. 🔄 Complete CI/CD Pipeline

The complete TrendStore CI/CD workflow is:

```text
Developer
   │
   ▼
GitHub
   │
   │ Webhook
   ▼
Jenkins
   │
   ├── Checkout Source
   │
   ├── Docker Build
   │
   ├── ECR Login
   │
   ├── Docker Tag
   │
   ├── Docker Push
   │
   ├── EKS Authentication
   │
   └── kubectl apply
          │
          ├── deployment.yaml
          ├── service.yaml
          └── ingress.yaml
                 │
                 ▼
               EKS
                 │
                 ▼
        AWS Load Balancer Controller
                 │
                 ▼
                ALB
                 │
                 ▼
              Internet
```

Whenever changes are pushed to GitHub, the Jenkins pipeline can automatically trigger the build and deployment process.

---

# 17. 🔐 IAM Permissions

The project uses AWS IAM roles and policies to provide controlled access to AWS resources.

Required AWS services include:

* Amazon ECR
* Amazon EKS
* IAM
* Amazon EC2
* Amazon VPC
* Elastic Load Balancing
* Amazon CloudWatch

The Jenkins execution identity requires permissions to:

* Authenticate to Amazon ECR
* Push Docker images
* Access the EKS cluster
* Update Kubernetes resources
* Retrieve AWS account information
* Update EKS kubeconfig

The EKS worker-node role requires permissions to pull private images from ECR.

The AWS Load Balancer Controller uses an IAM role associated with its Kubernetes service account.

Permissions should follow the principle of least privilege.

---

# 18. 🔑 Jenkins GitHub Integration

Jenkins is integrated with GitHub to automate source-code checkout and pipeline execution.

Typical configuration:

```text
Jenkins
   │
   ▼
GitHub Repository
   │
   ▼
Webhook
   │
   ▼
Jenkins Pipeline
```

Verify Git configuration:

```bash
git --version
```

Verify Jenkins:

```text
Jenkins Dashboard
→ Manage Jenkins
→ Tools
```

Configure:

* Git
* Docker
* AWS CLI
* kubectl

GitHub credentials should be configured securely in Jenkins Credentials Manager rather than hard-coded in the Jenkinsfile.

---

# 19. 🔔 GitHub Webhook

A GitHub webhook can be configured to trigger the Jenkins pipeline whenever code is pushed.

Example workflow:

```text
Developer
   │
   ▼
git push
   │
   ▼
GitHub
   │
   ▼
Webhook
   │
   ▼
Jenkins
   │
   ▼
Pipeline
   │
   ▼
Docker Build
   │
   ▼
ECR
   │
   ▼
EKS
```

This provides automated continuous integration and continuous deployment.

---

# 20. 📊 CloudWatch Monitoring

AWS CloudWatch can be used to monitor the TrendStore infrastructure and CI/CD environment.

Monitoring includes:

* Jenkins/EC2 infrastructure logs where configured
* EKS-related logs
* Application logs
* AWS Load Balancer metrics
* Infrastructure health
* Deployment status
* AWS service events

CloudWatch Logs can be used to investigate deployment failures and application issues.

---

# 21. 🩺 Kubernetes Health Checks

Check all Kubernetes resources:

```bash
kubectl get all
```

Check Pods:

```bash
kubectl get pods
```

Check Deployments:

```bash
kubectl get deployments
```

Check Services:

```bash
kubectl get svc
```

Check Ingress:

```bash
kubectl get ingress
```

Check endpoints:

```bash
kubectl get endpoints
```

Check cluster events:

```bash
kubectl get events \
  --sort-by=.metadata.creationTimestamp
```

Describe a Pod:

```bash
kubectl describe pod <POD-NAME>
```

Check application logs:

```bash
kubectl logs <POD-NAME>
```

---

# 22. 🐳 Docker Image Verification

Check local Docker images:

```bash
docker images
```

Check running containers:

```bash
docker ps
```

Check detailed image information:

```bash
aws ecr describe-images \
  --repository-name trend-app \
  --region ap-south-1
```

---

# 23. 🧹 Cleanup

Delete Kubernetes resources:

```bash
kubectl delete -f ingress.yaml
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml
```

Destroy Terraform-managed infrastructure only when it is safe to do so:

```bash
terraform destroy
```

> Do not run `terraform destroy` against shared or production infrastructure without confirming the resources managed by the Terraform state.

---

# 25. ✅ Project Validation

The final TrendStore deployment should satisfy:

```text
Terraform Infrastructure             ✅ Provisioned
AWS VPC                              ✅ Available
EC2 / Jenkins                        ✅ Running
GitHub Repository                    ✅ Available
Jenkins Pipeline                     ✅ Successful
Docker Image                         ✅ Built
ECR Image                            ✅ Available
EKS Cluster                          ✅ Available
EKS Nodes                            ✅ Ready
Kubernetes Deployment                ✅ Available
TrendStore Pods                      ✅ Running
ClusterIP Service                    ✅ Available
AWS Load Balancer Controller         ✅ Running
Ingress                              ✅ Created
Application Load Balancer            ✅ Created
ALB DNS                              ✅ Available
ALB ARN                              ✅ Available
Prometheus and Grafana               ✅ Monitoring
Application                          ✅ Accessible
```

---

# 26. 🎯 Project Outcome

The **TrendStore DevOps Project** demonstrates an end-to-end DevOps implementation on AWS.

The complete workflow integrates:

```text
GitHub
   ↓
Jenkins
   ↓
Docker Build
   ↓
Docker Hub
   ↓
Amazon EKS
   ↓
Kubernetes Deployment
   ↓
ClusterIP Service
   ↓
Kubernetes Ingress
   ↓
AWS Load Balancer Controller
   ↓
Application Load Balancer
   ↓
Internet
   ↓
TrendStore Application
```

Terraform provides Infrastructure as Code for provisioning AWS infrastructure, while Jenkins automates the application CI/CD workflow.

The project demonstrates practical implementation of:

* Source-code management
* Infrastructure as Code
* Docker containerization
* Container image management
* Continuous Integration
* Continuous Deployment
* Kubernetes orchestration
* Amazon EKS
* Kubernetes Ingress
* AWS Application Load Balancer
* IAM-based security
* AWS monitoring
* DevOps automation

The final solution provides a repeatable and automated deployment process for the TrendStore application running on Amazon EKS.

---

# 27. 📸 Project Documentation

The complete project steps, AWS configuration screenshots, Jenkins pipeline screenshots, Docker image screenshots, EKS screenshots, Kubernetes resource screenshots, ALB screenshots, and CloudWatch monitoring screenshots are documented and attached to this repository.

# 28. 📸 Project Submission
Application Load Balancer ARN: arn:aws:elasticloadbalancing:ap-south-1:526644151944:loadbalancer/app/k8s-trendstore-8a0598593b/cfd2a8c35e073bc6

---

# 🏆 Final Architecture

```text
                         ┌─────────────────────┐
                         │      Developer      │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │       GitHub        │
                         │   TrendStore Repo   │
                         └──────────┬──────────┘
                                    │
                                 Webhook
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │       Jenkins       │
                         │      CI/CD           │
                         └──────────┬──────────┘
                                    │
                    ┌───────────────┼───────────────┐
                    │               │               │
                    ▼               ▼               ▼
              Docker Build                       EKS Login
                    │               │               │
                    └───────────────┼───────────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │    Docker Build     │
                         │    Docker Image     │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     Amazon EKS      │
                         │   trend-cluster     │
                         └──────────┬──────────┘
                                    │
                         ┌──────────┴──────────┐
                         │                     │
                         ▼                     ▼
                 Kubernetes Deployment    ClusterIP Service
                         │                     │
                         ▼                     │
                  TrendStore Pods ◄────────────┘
                         │
                         ▼
                 Kubernetes Ingress
                         │
                         ▼
              AWS Load Balancer Controller
                         │
                         ▼
               Application Load Balancer
                         │
                         ▼
                      Internet
                         │
                         ▼
                 TrendStore Web App
```

---

# 🚀 Final Result

```text
Source Code       → GitHub
Infrastructure    → Terraform
CI/CD             → Jenkins
Container         → Docker
Registry          → Docker Hub
Orchestration     → Amazon EKS
Deployment        → Kubernetes
External Access   → AWS ALB
Ingress           → AWS Load Balancer Controller
Monitoring        → Prometheus and Grafana
Application       → TrendStore
```

**TrendStore is deployed using a complete AWS DevOps workflow with automated CI/CD, containerization, Kubernetes orchestration, ALB-based external access, Infrastructure as Code, and cloud monitoring.**
