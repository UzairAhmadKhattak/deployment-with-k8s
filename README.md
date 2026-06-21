
# 🚀 RKE2 Local Kubernetes Deployment (FastAPI / Knowledge Assistant)

This project demonstrates a **local Kubernetes deployment using RKE2 + containerd + NGINX Ingress Controller**.  
It is designed as a learning project to understand real-world container orchestration, service networking, and deployment workflows.

---

## 📦 Overview

This setup covers:

- Docker image build & packaging
- Importing images into RKE2 containerd
- Kubernetes Secrets management
- Deployment via Kubernetes manifests
- Internal cluster networking (ClusterIP services)
- External access using NGINX Ingress Controller

---

## 🏗️ Architecture

- **Container Runtime:** containerd (via RKE2)
- **Orchestration:** Kubernetes (RKE2)
- **Ingress Controller:** NGINX Ingress
- **Service Type:** ClusterIP (internal communication)
- **External Access:** Ingress routing

---

## 🧱 Deployment Steps

### 1. Build Docker Image

```bash
docker build -t knowledge-assistant-image:latest .
````

---

### 2. Save Image as TAR File

```bash
docker save knowledge-assistant-image:latest -o image.tar
```

---

### 3. Import Image into RKE2 containerd

```bash
sudo /var/lib/rancher/rke2/bin/ctr \
  --address /run/rke2/containerd/containerd.sock \
  -n k8s.io images import image.tar
```

---

### 4. Create Kubernetes Secret

#### Option A: Inline Secret

```bash
kubectl create secret generic app-secret \
  --from-literal=DATABASE_URL="your_database_url_here"
```

---

### 5. Deploy Application

```bash
kubectl apply -f deployment.yaml
```

---

### 6. Image Pull Policy

Since the image is locally imported into RKE2:

```yaml
imagePullPolicy: Never
```

or

```yaml
imagePullPolicy: IfNotPresent
```

---

### 7. Service Configuration (ClusterIP)

* The service is configured as **ClusterIP**
* Ensures **internal-only communication** within the Kubernetes cluster
* Used as a backend for the Ingress Controller

---

### 8. Ingress (NGINX Controller)

* NGINX Ingress is used to expose services externally
* Handles routing based on host/path rules
* Acts as a gateway to internal ClusterIP services

---

## ☁️ Note on Cloud-Native Transition

This deployment is designed for **local RKE2 environments**.

If moved to a **cloud-native setup (AWS EKS / GKE / AKS)**, the following changes would typically apply:

* Use a **container registry (ECR / Docker Hub / GCR)** instead of local image import
* Remove manual `ctr image import`
* Use `imagePullPolicy: Always`
* Replace local secrets with managed secret services (AWS Secrets Manager, etc.)
* Integrate cloud load balancers instead of local ingress setup

---

## 🎯 Learning Goals

This project helped me understand:

* Kubernetes deployment lifecycle
* RKE2 + containerd image management
* Service discovery inside clusters
* Ingress-based routing with NGINX
* Secret management in Kubernetes
* Differences between local vs cloud-native deployments

---

## 👨‍💻 About This Project

This is a **hands-on DevOps learning project** focused on backend deployment workflows using Kubernetes.

It reflects practical experience with:

* Docker
* Kubernetes (RKE2)
* NGINX Ingress
* CI/CD deployment patterns (local-first approach)

---

## ⭐ Why This Matters

This project demonstrates the ability to:

* Deploy real-world applications on Kubernetes
* Understand container lifecycle in RKE2 environments
* Configure networking using ClusterIP + Ingress
* Work with infrastructure-level debugging and setup


