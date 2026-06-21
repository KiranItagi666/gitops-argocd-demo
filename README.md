# 🚀 End-to-End GitOps CI/CD Pipeline Using Kubernetes, Helm, ArgoCD & GitHub Actions

This project demonstrates a complete beginner-friendly modern DevOps workflow using:

* Kubernetes (Kind Cluster)
* Docker
* DockerHub
* GitHub Actions
* Helm
* ArgoCD
* GitOps Workflow
* Node.js

The project shows how to build a complete CI/CD pipeline that automatically deploys applications to Kubernetes using GitOps principles.

---

# 📊 Project Status

✅ Dockerized Application

✅ Kubernetes Deployment

✅ Helm Packaging

✅ GitHub Actions CI Pipeline

✅ ArgoCD Continuous Deployment

✅ GitOps Workflow

🚧 Future Enhancements

* Dynamic Image Tag Automation
* Prometheus & Grafana Monitoring
* Ingress Controller
* Multi-Environment GitOps
* Terraform Integration
* Cloud Deployments (EKS/GKE/AKS)

---

# 📋 Prerequisites

Ensure the following tools are installed:

* Git
* Node.js (v20 or later)
* Docker
* kubectl
* Kind
* Helm
* GitHub Account
* DockerHub Account

Verify installations:

```bash
git --version

node --version

docker --version

kubectl version --client

kind version

helm version
```

---

# 📌 Architecture

```text
Developer Pushes Code
        ↓
GitHub Repository
        ↓
GitHub Actions CI Pipeline
        ↓
Docker Image Build
        ↓
DockerHub Push
        ↓
Helm Charts
        ↓
ArgoCD Watches Git Repository
        ↓
Kubernetes Cluster Sync
        ↓
Automatic Deployment 🚀
```

---

# 📂 Project Structure

```text
gitops-argocd-demo/
│
├── .github/
│   └── workflows/
│       └── docker-build.yml
│
├── gitops-argocd-demo-chart/
│
├── k8s/
│
├── Dockerfile
├── package.json
├── package-lock.json
├── server.js
└── README.md
```

---

# ⚙️ Technologies Used

* Kubernetes
* Kind
* Docker
* DockerHub
* GitHub Actions
* Helm
* ArgoCD
* GitOps
* Node.js

---

# 🚀 STEP 1 — Create GitHub Repository

Create a new GitHub repository.

Example:

```text
gitops-argocd-demo
```

Clone repository:

```bash
git clone https://github.com/<YOUR_GITHUB_USERNAME>/<YOUR_REPOSITORY>.git
```

Enter repository:

```bash
cd <YOUR_REPOSITORY>
```

---

# 🚀 STEP 2 — Create Node.js Application

Initialize project:

```bash
npm init -y
```

Install Express:

```bash
npm install express
```

Create application file:

```bash
touch server.js
```

Add the following code inside server.js:

```javascript
const express = require('express');

const app = express();

app.get('/', (req, res) => {
  res.send('Hello from Kubernetes + ArgoCD + GitOps!');
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

Update package.json:

```json
"scripts": {
  "start": "node server.js"
}
```

Run application:

```bash
npm start
```

Verify:

```text
http://localhost:3000
```

---

# 🚀 STEP 3 — Create Dockerfile

Create Dockerfile:

```bash
touch Dockerfile
```

Add:

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

---

# 🚀 STEP 4 — Build Docker Image

Build image:

```bash
docker build -t gitops-argocd-demo:v1 .
```

Verify images:

```bash
docker images
```

Run container:

```bash
docker run -d -p 3000:3000 gitops-argocd-demo:v1
```

Verify:

```bash
docker ps
```

---

# 🚀 STEP 5 — Push Docker Image To DockerHub

Login:

```bash
docker login
```

Tag image:

```bash
docker tag gitops-argocd-demo:v1 <DOCKERHUB_USERNAME>/gitops-argocd-demo:v1
```

Push image:

```bash
docker push <DOCKERHUB_USERNAME>/gitops-argocd-demo:v1
```

Verify the image appears in DockerHub.

---

# 🚀 STEP 6 — Install Kind Kubernetes Cluster

Verify Kind installation:

```bash
kind version
```

Create cluster:

```bash
kind create cluster --name gitops-demo
```

Verify:

```bash
kubectl get nodes
```

Expected output:

```text
gitops-demo-control-plane   Ready
```

---

# 🚀 STEP 7 — Create Kubernetes Deployment

Create:

```text
k8s/deployment.yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: gitops-argocd-demo

spec:
  replicas: 1

  selector:
    matchLabels:
      app: gitops-argocd-demo

  template:
    metadata:
      labels:
        app: gitops-argocd-demo

    spec:
      containers:
      - name: gitops-argocd-demo

        image: gitops-argocd-demo:v1

        imagePullPolicy: Never

        ports:
        - containerPort: 3000
```

Apply deployment:

```bash
kubectl apply -f k8s/deployment.yaml
```

Verify:

```bash
kubectl get deployments

kubectl get pods
```

---

# 🚀 STEP 8 — Create Kubernetes Service

Create:

```text
k8s/service.yaml
```

```yaml
apiVersion: v1
kind: Service

metadata:
  name: gitops-argocd-demo-service

spec:
  selector:
    app: gitops-argocd-demo

  type: NodePort

  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
```

Apply:

```bash
kubectl apply -f k8s/service.yaml
```

Verify:

```bash
kubectl get svc

kubectl describe svc gitops-argocd-demo-service
```

---

# 🚀 STEP 9 — Install Helm

Verify installation:

```bash
helm version
```

Create chart:

```bash
helm create gitops-argocd-demo-chart
```

Verify structure:

```bash
tree gitops-argocd-demo-chart
```

---

# 🚀 STEP 10 — Configure values.yaml

Open:

```text
gitops-argocd-demo-chart/values.yaml
```

Update:

```yaml
replicaCount: 1

image:
  repository: <DOCKERHUB_USERNAME>/gitops-argocd-demo
  pullPolicy: IfNotPresent
  tag: v1

service:
  type: NodePort
  port: 3000
```

Verify:

```bash
helm template gitops-demo .
```
# 🚀 STEP 11 — Update Deployment Template

Open:

```text
gitops-argocd-demo-chart/templates/deployment.yaml
```

Ensure the image section looks like:

```yaml
image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

For a beginner demo application, remove the default probes generated by Helm:

```yaml
livenessProbe:
readinessProbe:
```

This prevents accidental failures during initial learning and testing.

Verify rendered manifests:

```bash
helm template gitops-demo .
```

---

# 🚀 STEP 12 — Deploy Using Helm

Install the Helm release:

```bash
helm install gitops-demo ./gitops-argocd-demo-chart
```

Verify release:

```bash
helm list
```

Check deployments:

```bash
kubectl get deployments
```

Check pods:

```bash
kubectl get pods
```

Check services:

```bash
kubectl get svc
```

Expected:

```text
Deployment Running

Pod Running

NodePort Service Created
```

---

# 🚀 STEP 13 — Create GitHub Actions Workflow

Create workflow directory:

```bash
mkdir -p .github/workflows
```

Create workflow file:

```bash
touch .github/workflows/docker-build.yml
```

Add:

```yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:

  docker:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Login to DockerHub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker Image
        run: |
          docker build -t ${{ secrets.DOCKER_USERNAME }}/gitops-argocd-demo:latest .

      - name: Push Docker Image
        run: |
          docker push ${{ secrets.DOCKER_USERNAME }}/gitops-argocd-demo:latest
```

Commit the workflow:

```bash
git add .

git commit -m "Add GitHub Actions pipeline"

git push origin main
```

---

# 🚀 STEP 14 — Configure GitHub Secrets

Navigate to:

```text
Repository
→ Settings
→ Secrets and Variables
→ Actions
```

Create:

```text
DOCKER_USERNAME

DOCKER_PASSWORD
```

Example:

```text
DOCKER_USERNAME = johndoe

DOCKER_PASSWORD = ************
```

---

# 🚀 STEP 15 — Verify CI Pipeline

Push code:

```bash
git add .

git commit -m "Trigger CI Pipeline"

git push origin main
```

Open:

```text
GitHub Repository
→ Actions
```

Verify:

```text
✓ Checkout Code

✓ Docker Login

✓ Docker Build

✓ Docker Push
```

Verify the image appears in DockerHub.

---

# 🚀 STEP 16 — Install ArgoCD

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

Wait until all components are Running.

Expected components:

```text
argocd-server

argocd-repo-server

argocd-application-controller

argocd-dex-server
```

---

# 🚀 STEP 17 — Configure ArgoCD

Enable insecure mode for local access:

```bash
kubectl patch configmap argocd-cmd-params-cm \
-n argocd \
--type merge \
-p '{"data":{"server.insecure":"true"}}'
```

Restart server:

```bash
kubectl rollout restart deployment argocd-server -n argocd
```

Verify:

```bash
kubectl get pods -n argocd
```

---

# 🚀 STEP 18 — Access ArgoCD UI

Port forward:

```bash
kubectl port-forward svc/argocd-server \
-n argocd \
9090:80
```

Open browser:

```text
http://localhost:9090
```

---

# 🚀 STEP 19 — Get ArgoCD Admin Password

Retrieve password:

```bash
kubectl get secret argocd-initial-admin-secret \
-n argocd \
-o jsonpath="{.data.password}" | base64 -d
```

Login:

```text
Username: admin

Password: <OUTPUT>
```

---

# 🚀 STEP 20 — Create ArgoCD Application

Create:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application

metadata:
  name: gitops-demo
  namespace: argocd

spec:
  project: default

  source:
    repoURL: https://github.com/<YOUR_GITHUB_USERNAME>/<YOUR_REPOSITORY>.git
    targetRevision: main
    path: gitops-argocd-demo-chart

  destination:
    server: https://kubernetes.default.svc
    namespace: default

  syncPolicy:
    automated:
      prune: true
      selfHeal: true

    syncOptions:
      - CreateNamespace=true
```

Apply:

```bash
kubectl apply -f application.yaml
```

Verify:

```bash
kubectl get applications -n argocd
```

---

# 🚀 STEP 21 — Verify GitOps Deployment

Open ArgoCD Dashboard.

Expected:

```text
Status: Healthy

Status: Synced
```

Verify Kubernetes resources:

```bash
kubectl get deployments

kubectl get pods

kubectl get svc
```

---

# 🚀 STEP 22 — Test GitOps Synchronization

Update values.yaml:

```yaml
replicaCount: 1
```

Change to:

```yaml
replicaCount: 2
```

Commit:

```bash
git add .

git commit -m "Scale application to 2 replicas"

git push origin main
```

Watch ArgoCD:

```text
OutOfSync
↓
Syncing
↓
Healthy
```

Verify:

```bash
kubectl get pods
```

Expected:

```text
2 Running Pods
```

---

# ⚠️ Important GitOps Note

ArgoCD watches Git repositories, not DockerHub.

A typical production GitOps workflow is:

```text
Code Change
      ↓
GitHub Actions Build
      ↓
DockerHub Push
      ↓
Update Helm values.yaml Image Tag
      ↓
Commit Changes To Git
      ↓
ArgoCD Detects Git Change
      ↓
Kubernetes Rollout
```

Simply pushing a Docker image does not trigger ArgoCD.

The Git repository remains the single source of truth.

---

# 📌 Key Concepts Learned

* Kubernetes Architecture
* Docker Containerization
* Kubernetes Deployments
* Kubernetes Services
* ReplicaSets
* Helm Charts
* Helm Releases
* GitHub Actions CI/CD
* DockerHub Integration
* ArgoCD
* GitOps Workflow
* Self-Healing Infrastructure
* Declarative Infrastructure
* Continuous Deployment

---

# 🚀 Final Outcome

You now have:

✅ Dockerized Application

✅ Kubernetes Cluster

✅ Helm-Based Deployment

✅ GitHub Actions CI Pipeline

✅ DockerHub Integration

✅ ArgoCD Continuous Deployment

✅ GitOps Automation

✅ Self-Healing Infrastructure

✅ Automated Kubernetes Synchronization

---

# 📌 Future Improvements

* Dynamic Image Tag Automation
* Prometheus Monitoring
* Grafana Dashboards
* Ingress Controller
* HTTPS/TLS
* Terraform
* AWS EKS
* Azure AKS
* Google GKE
* Multi-Environment GitOps
* Canary Deployments
* Blue-Green Deployments

---

# 👨‍💻 Author

Kiran Itagi

Replace this section with your own details before publishing or forking.

GitHub:

```text
https://github.com/KiranItagi666
```

---

# ⭐ Support

If this project helped you learn GitOps:

* Star the repository
* Fork the repository
* Share it with the DevOps community
* Contribute improvements

Happy Learning! 🚀
