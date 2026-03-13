# 🚀 Enterprise-Grade GitOps: Kubernetes, Helm, Kustomize & ArgoCD

![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Helm](https://img.shields.io/badge/Helm-0F1689?style=for-the-badge&logo=Helm&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-EF7B4D?style=for-the-badge&logo=argo&logoColor=white)
![GitOps](https://img.shields.io/badge/GitOps-000000?style=for-the-badge)

## 📌 Project Overview
Welcome to the **Kubernetes GitOps Deployment** repository. This project demonstrates a real-world, industry-standard approach to deploying and managing applications (specifically a Grade Submission API) on Kubernetes. 

Instead of manual deployments, this project leverages **GitOps principles**:
* **ArgoCD** for Continuous Delivery.
* **Helm** for templating and packaging.
* **Kustomize** for environment-specific configuration management.

---

## 🏗️ Architecture & Workflow
The workflow follows the GitOps pull-model:
1.  **Code Changes** are pushed to GitHub.
2.  **ArgoCD** monitors the repository for any configuration changes.
3.  **Automatic Sync:** ArgoCD pulls the Helm charts/Kustomize overlays and applies them to the Kubernetes cluster to match the desired state.

> **Note:** Add your architecture diagram here! 
> `![Architecture](./images/architecture-diagram.png)`

---

## 📂 Repository Structure
* **`/argocd/applications`**: Contains ArgoCD Application manifests (The "App of Apps" pattern).
* **`/helm/grade-submission`**: Custom Helm charts developed for the Grade Submission API.
* **`/kustomize`**: Base and Overlay configurations (Dev/Prod) to manage environment variations.
* **`/gitops`**: Core configuration files for the GitOps pipeline.

---

## 🚀 Getting Started


### 1. Install ArgoCD on your Cluster
```
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
kubectl create namespace argocd
helm install argocd argo/argo-cd --namespace argocd --version 7.7.0
```

### 2. Access ArgoCD Dashboard
Port-forward the service to access the UI locally:

```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

### 3. Retrieve Credentials
Get the initial admin password (decoded):

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### 4. API Calls
Once the GitOps pipeline syncs the application, you can interact with the Grade Submission API.

Note: Windows users should use Git Bash for these commands.

Here are commands that you can use to add grades to the Grade Submission API. **Windows Users should use Git Bash**.

```bash
curl -X POST http://localhost:<port>/grades \
  -H "Content-Type: application/json" \
  -d '{"name": "Harry", "subject": "Defense Against Dark Arts", "score": 95}'

curl -X POST http://localhost:<port>/grades \
  -H "Content-Type: application/json" \
  -d '{"name": "Ron", "subject": "Charms", "score": 82}'

curl -X POST http://localhost:<port>/grades \
  -H "Content-Type: application/json" \
  -d '{"name": "Hermione", "subject": "Potions", "score": 98}'
```

To verify, you can get all grades with:
```bash
curl http://localhost:<port>/grades
```
