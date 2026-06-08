# 🚀 Deploy Flask Backend Application on Amazon EKS

## 📌 Project Overview

This project demonstrates how to deploy a containerized Flask backend application on Amazon Elastic Kubernetes Service (EKS).

The application source code was cloned from GitHub, containerized using Docker, stored in Amazon Elastic Container Registry (ECR), and deployed to an EKS cluster using Kubernetes manifests.

---

## 🏗️ Architecture

```text
GitHub Repository
        │
        ▼
EC2 Instance
        │
        ▼
Docker Build
        │
        ▼
Amazon ECR
        │
        ▼
Amazon EKS Cluster
        │
        ▼
Kubernetes Deployment
        │
        ▼
Flask Backend Pods
```

---

## 🛠️ AWS Services Used

- Amazon EC2
- Amazon EKS
- Amazon ECR
- IAM
- Docker
- Kubernetes
- kubectl
- eksctl
- Git

---

## 🎯 Project Objectives

- Create an EKS Cluster
- Configure IAM Roles
- Clone Backend Application
- Build Docker Image
- Push Image to ECR
- Create Kubernetes Manifests
- Deploy Application using kubectl
- Verify Pods and Nodes

---

## Step 1: Launch EC2 Instance

Created an Amazon Linux 2023 EC2 instance to manage the Kubernetes cluster.

### Installed eksctl

```bash
eksctl version
```

---

## Step 2: Configure IAM Role

Created:

```text
nextwork-eks-instance-role
```

Attached:

```text
AdministratorAccess
```

Attached the IAM role to the EC2 instance.

---

## Step 3: Create Amazon EKS Cluster

```bash
eksctl create cluster \
--name nextwork-eks-cluster \
--nodegroup-name nextwork-nodegroup \
--node-type t3.micro \
--nodes 3 \
--nodes-min 1 \
--nodes-max 3
```

### Cluster Configuration

| Property | Value |
|-----------|---------|
| Cluster Name | nextwork-eks-cluster |
| Node Group | nextwork-nodegroup |
| Node Type | t3.micro |
| Nodes | 3 |
| Kubernetes Version | 1.33 |

---

## Step 4: Clone Backend Repository

Installed Git and cloned the Flask backend application repository.

```bash
git clone <repository-url>
```

---

## Step 5: Build Docker Image

Installed Docker and built the application image.

```bash
docker build -t nextwork-flask-backend .
```

---

## Step 6: Push Docker Image to Amazon ECR

Created ECR Repository:

```bash
aws ecr create-repository \
--repository-name nextwork-flask-backend
```

Tagged and pushed image:

```bash
docker tag nextwork-flask-backend:latest <ecr-uri>:latest

docker push <ecr-uri>:latest
```

---

## Step 7: Create Kubernetes Deployment Manifest

### flask-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nextwork-flask-backend

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nextwork-flask-backend

  template:
    metadata:
      labels:
        app: nextwork-flask-backend

    spec:
      containers:
      - name: nextwork-flask-backend
        image: <ecr-image-uri>
        ports:
        - containerPort: 8080
```

### Purpose

- Deploy Flask Backend
- Maintain 3 replicas
- Pull image from ECR

---

## Step 8: Create Kubernetes Service Manifest

### flask-service.yaml

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nextwork-flask-backend

spec:
  selector:
    app: nextwork-flask-backend

  type: NodePort

  ports:
  - port: 8080
    targetPort: 8080
```

### Purpose

- Expose backend application
- Route traffic to backend pods
- Allow external access through NodePort

---

## Step 9: Configure kubectl

```bash
aws eks update-kubeconfig \
--name nextwork-eks-cluster \
--region <region>
```

---

## Step 10: Deploy Application

```bash
kubectl apply -f flask-deployment.yaml

kubectl apply -f flask-service.yaml
```

---

## Verification

### Check Nodes

```bash
kubectl get nodes
```

### Check Pods

```bash
kubectl get pods
```

### Check Services

```bash
kubectl get svc
```

---

## Screenshots

### EKS Cluster

![EKS Cluster](screenshots/eks-cluster-created.png)

### ECR Repository

![ECR Repository](screenshots/ecr-repository.png)

### Deployment Manifest

![Deployment](screenshots/deployment-manifest.png)

### Service Manifest

![Service](screenshots/service-manifest.png)

### kubectl Apply

![kubectl](screenshots/kubectl-apply.png)

### EKS Nodes

![Nodes](screenshots/eks-nodes.png)

### Pod Events

![Pods](screenshots/pod-events.png)

---

## Key Learnings

- Learned Amazon EKS cluster creation.
- Built and containerized applications using Docker.
- Stored images in Amazon ECR.
- Created Kubernetes Deployment and Service manifests.
- Deployed applications using kubectl.
- Verified workloads through the EKS console.
- Understood Pods, Deployments, Services, and Node Groups.

---

## Cleanup

Delete EKS Cluster

```bash
eksctl delete cluster \
--name nextwork-eks-cluster
```

Terminate EC2 Instance and delete ECR Repository to avoid AWS charges.

---

## Author

**Jayasri K**

Aspiring Cloud & DevOps Engineer ☁️