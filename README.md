# ArgoCD CLI GitOps Deployment Using Kubernetes

### Automating Kubernetes Application Deployment with GitHub and ArgoCD

This project demonstrates how to deploy an **HTTPD web application** on a **Kubernetes cluster** using **ArgoCD CLI** and the **GitOps** methodology. The Kubernetes manifests are stored in a GitHub repository, and ArgoCD continuously monitors the repository for changes. Whenever updates are pushed to GitHub, ArgoCD automatically synchronizes the Kubernetes cluster with the latest desired state.

---

## 📌 Project Overview

This project showcases the complete GitOps workflow using:

- Kubernetes
- ArgoCD
- GitHub
- HTTPD (Apache Web Server)

The application is deployed from a GitHub repository, managed by ArgoCD, and automatically updated whenever changes are committed to GitHub.

---

## 🚀 Features

- Deploy HTTPD application on Kubernetes
- Install and configure ArgoCD
- Deploy applications using ArgoCD CLI
- GitOps-based Continuous Deployment
- Automatic Synchronization
- Automatic Rollout after GitHub commits
- Easy application management through ArgoCD Dashboard

---

## 🏗️ Project Architecture

```
                 Developer
                     │
                     ▼
             Push Code to GitHub
                     │
                     ▼
          GitHub Repository (YAML)
                     │
                     ▼
                 ArgoCD Server
                     │
          Detects Repository Changes
                     │
                     ▼
           Synchronizes Kubernetes
                     │
                     ▼
          HTTPD Application Running
```

---

## 📂 Repository Structure

```
demo-argocd/
│
├── deployment.yaml
├── service.yaml
├── index.html
└── README.md
```

---

## 🛠️ Prerequisites

Before starting, ensure you have:

- Kubernetes Cluster
- kubectl
- Git
- GitHub Account
- ArgoCD Installed
- ArgoCD CLI
- Internet Connection

---

# Step 1 - Create Deployment Manifest

Create a Deployment file named:

```
deployment.yaml
```

Apply the deployment:

```bash
kubectl apply -f deployment.yaml
```

Verify deployment:

```bash
kubectl get deployments
kubectl get pods
```

---

# Step 2 - Create Service

Create:

```
service.yaml
```

Deploy the service:

```bash
kubectl apply -f service.yaml
```

Verify:

```bash
kubectl get svc
```

---

# Step 3 - Push Files to GitHub

Initialize Git Repository

```bash
git init
```

Add files

```bash
git add .
```

Commit

```bash
git commit -m "Initial Commit"
```

Create Main Branch

```bash
git branch -M main
```

Add Remote Repository

```bash
git remote add origin https://github.com/<your-username>/demo-argocd.git
```

Push Repository

```bash
git push -u origin main
```

---

# Step 4 - Install ArgoCD

Create Namespace

```bash
kubectl create namespace argocd
```

Install ArgoCD

```bash
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

---

# Step 5 - Verify Installation

Check Pods

```bash
kubectl get pods -n argocd
```

Check Services

```bash
kubectl get svc -n argocd
```

---

# Step 6 - Expose ArgoCD Server

```bash
kubectl patch svc argocd-server \
-n argocd \
-p '{"spec":{"type":"NodePort"}}'
```

---

# Step 7 - Retrieve Initial Admin Password

```bash
kubectl get secret argocd-initial-admin-secret \
-n argocd \
-o jsonpath="{.data.password}" | base64 -d
```

---

# Step 8 - Install ArgoCD CLI

Download

```bash
curl -sSL -o argocd \
https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
```

Give Execute Permission

```bash
chmod +x argocd
```

Move Binary

```bash
sudo mv argocd /usr/local/bin/
```

Verify Installation

```bash
argocd version
```

---

# Step 9 - Login to ArgoCD

```bash
argocd login <NodeIP>:<NodePort> \
--username admin \
--password <password> \
--insecure
```

---

# Step 10 - Create ArgoCD Application

```bash
argocd app create httpd-demo \
--repo https://github.com/<your-username>/demo-argocd.git \
--path . \
--dest-server https://kubernetes.default.svc \
--dest-namespace default
```

---

# Step 11 - Enable Automatic Sync

```bash
argocd app set httpd-demo \
--sync-policy automated
```

---

# Step 12 - Verify Application

```bash
argocd app get httpd-demo
```

Expected Status

```
Health : Healthy
Sync   : Synced
```

---

# Step 13 - Verify Deployment

```bash
kubectl get pods
```

```bash
kubectl get svc
```

Open your browser:

```
http://<NodeIP>:<NodePort>
```

You should see the HTTPD default page.

---

# Step 14 - Demonstrate GitOps

Modify the `index.html` file.

Example:

```html
<h1>Welcome to ArgoCD GitOps Demo</h1>
<h3>Sachin Gadekar</h3>
<p>Application deployed using Kubernetes + ArgoCD</p>
```

Commit the changes.

```bash
git add .
git commit -m "Updated index page"
git push origin main
```

ArgoCD automatically detects the changes and synchronizes the Kubernetes cluster without requiring manual deployment.

---

## 📸 Screenshots

Add screenshots for:

- Deployment YAML
- Service YAML
- GitHub Repository
- ArgoCD Installation
- ArgoCD Dashboard
- CLI Login
- Application Status
- Sync Status
- Kubernetes Pods
- Browser Output
- Updated Webpage after Git Commit

---

## 🧰 Technologies Used

- Kubernetes
- ArgoCD
- GitHub
- Git
- ArgoCD CLI
- YAML
- HTTPD (Apache Web Server)
- Linux

---

## 📖 GitOps Workflow

```
Developer
      │
      ▼
Push Changes to GitHub
      │
      ▼
ArgoCD Detects Changes
      │
      ▼
Automatic Synchronization
      │
      ▼
Deploys to Kubernetes
      │
      ▼
Updated Application
```

---

## 🎯 Learning Outcomes

- Understand GitOps Principles
- Deploy Applications with ArgoCD
- Configure ArgoCD CLI
- Connect GitHub Repository
- Enable Automatic Synchronization
- Manage Kubernetes Deployments
- Monitor Applications using ArgoCD Dashboard

---

## ✅ Result

Successfully deployed an HTTPD application on Kubernetes using ArgoCD CLI. The application is continuously synchronized with the GitHub repository, enabling automated GitOps-based deployments and updates.

---

## 👨‍💻 Author

**Sachin Gadekar**

GitHub: https://github.com/sgadekar8181

LinkedIn: https://www.linkedin.com/in/sachin-gadekar/
