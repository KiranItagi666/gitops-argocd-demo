# 🚀 End-to-End GitOps CI/CD Pipeline Using Kubernetes, Helm, ArgoCD & GitHub Actions

This project demonstrates a complete GitOps workflow using:

* Node.js + Express
* Docker
* DockerHub
* Kubernetes (Kind)
* Helm
* GitHub Actions
* ArgoCD

The goal is to automatically deploy application changes to Kubernetes through GitOps principles.

---

## Architecture

```text
Developer
    │
    ▼
Git Push
    │
    ▼
GitHub Repository
    │
    ▼
GitHub Actions
(Build & Push Docker Image)
    │
    ▼
DockerHub
    │
    ▼
ArgoCD
    │
    ▼
Kubernetes Cluster
```

---

## Project Structure

```text
gitops-argocd-demo/
│
├── .github/
│   └── workflows/
│       └── docker-build.yml
│
├── gitops-argocd-demo-chart/
│   ├── templates/
│   ├── Chart.yaml
│   └── values.yaml
│
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
│
├── Dockerfile
├── package.json
├── package-lock.json
├── server.js
└── README.md
```

---

## Prerequisites

Install the following tools:

* Git
* Docker Desktop
* WSL2
* kubectl
* Kind
* Helm
* ArgoCD CLI (optional)

---

## Step 1: Clone Repository

```bash
git clone https://github.com/KiranItagi666/gitops-argocd-demo.git

cd gitops-argocd-demo
```

---

## Step 2: Install Dependencies

```bash
npm install
```

Run locally:

```bash
npm start
```

Application runs on:

```text
http://localhost:3000
```

---

## Step 3: Build Docker Image

```bash
docker build -t gitops-argocd-demo:v1 .
```

Run locally:

```bash
docker run -d -p 3000:3000 --name gitops-demo gitops-argocd-demo:v1
```

Verify:

```bash
docker ps
```

---

## Step 4: Push Image to DockerHub

Tag image:

```bash
docker tag gitops-argocd-demo:v1 kviondocker/gitops-argocd-demo:v1
```

Push:

```bash
docker push kviondocker/gitops-argocd-demo:v1
```

---

## Step 5: Create Kind Cluster

```bash
kind create cluster --name gitops-demo
```

Verify:

```bash
kubectl get nodes
```

Expected:

```text
gitops-demo-control-plane Ready
```

---

## Step 6: Deploy Using Helm

Install chart:

```bash
cd gitops-argocd-demo-chart

helm install gitops-demo .
```

Verify:

```bash
kubectl get deployments

kubectl get pods

kubectl get svc
```

---

## Step 7: Configure GitHub Actions

Create GitHub Secrets:

### DOCKER_USERNAME

```text
kviondocker
```

### DOCKER_PASSWORD

```text
DockerHub Password or Access Token
```

Workflow automatically:

* Builds Docker image
* Pushes image to DockerHub

File:

```text
.github/workflows/docker-build.yml
```

---

## Step 8: Install ArgoCD

Create namespace:

```bash
kubectl create namespace argocd
```

Install ArgoCD:

```bash
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Verify:

```bash
kubectl get pods -n argocd
```

---

## Step 9: Access ArgoCD UI

Port forward:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open:

```text
https://localhost:8080
```

Get admin password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d
```

Username:

```text
admin
```

---

## Step 10: Create ArgoCD Application

Repository:

```text
https://github.com/KiranItagi666/gitops-argocd-demo
```

Path:

```text
gitops-argocd-demo-chart
```

Cluster:

```text
https://kubernetes.default.svc
```

Namespace:

```text
default
```

Enable:

* Auto Sync
* Prune Resources
* Self Heal

---

## GitOps Validation

Update:

```yaml
replicaCount: 1
```

to:

```yaml
replicaCount: 2
```

Commit and push:

```bash
git add .

git commit -m "Scale application to 2 replicas"

git push
```

ArgoCD automatically detects the change and updates Kubernetes.

Verify:

```bash
kubectl get pods
```

Expected:

```text
2 Running Pods
```

without executing:

```bash
helm upgrade
kubectl apply
```

---

## Technologies Used

* Node.js
* Express
* Docker
* DockerHub
* Kubernetes
* Kind
* Helm
* GitHub Actions
* ArgoCD
* GitOps

---

## Learning Outcomes

This project demonstrates:

* Containerization using Docker
* Kubernetes Deployments and Services
* Helm Packaging
* CI Pipeline using GitHub Actions
* DockerHub Integration
* GitOps Deployment using ArgoCD
* Automated Kubernetes Synchronization

---

## Author

Kiran Itagi

GitHub:
https://github.com/KiranItagi666
