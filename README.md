


# 🗳️ Online Voting App | DevOps Deployment Guide 🚀

A modern, scalable, and secure online voting application deployed using DevOps best practices.  
**Built with**: Docker 🐳, Kubernetes ☸️, ArgoCD 🔄, and Kubernetes Dashboard 📊.

---

## 📦 Tech Stack
- **Containerization**: Docker
- **Orchestration**: Kubernetes (k3s/Minikube)
- **GitOps**: ArgoCD
- **Monitoring**: Kubernetes Dashboard
- **Code Management**: Git
- **Networking**: kube-proxy, Service/Ingress

---

## 🛠️ Prerequisites
1. Docker installed: [Get Docker](https://docs.docker.com/get-docker/)
2. Kubernetes cluster (kind): https://kind.sigs.k8s.io/docs/user/quick-start/
3. `kubectl` CLI: [Install kubectl](https://kubernetes.io/docs/tasks/tools/)
4. ArgoCD CLI: [Install ArgoCD CLI](https://argo-cd.readthedocs.io/en/stable/cli_installation/)
5. Git: [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

---

## 🚀 Deployment Steps

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/online-voting-app.git
cd online-voting-app
```

### 2. Build Docker Images
```bash
# Build Frontend
docker build -t your-registry/voting-frontend:latest -f frontend/Dockerfile .

# Build Backend
docker build -t your-registry/voting-backend:latest -f backend/Dockerfile .
```

### 3. Push Images to Container Registry
```bash
docker push your-registry/voting-frontend:latest
docker push your-registry/voting-backend:latest
```
> ℹ️ Replace `your-registry` with Docker Hub, AWS ECR, or private registry.

---

### 4. Kubernetes Setup
#### Create Namespaces
```bash
kubectl create namespace voting-app
kubectl create namespace argocd
```

#### Apply Kubernetes Manifests
```bash
kubectl apply -f k8s/manifests/ -n voting-app
```

---

### 5. ArgoCD Setup
#### Install ArgoCD in Cluster
```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

#### Deploy Application via ArgoCD
```bash
kubectl apply -f k8s/argocd/applications/voting-app.yaml -n argocd
```

#### Access ArgoCD Dashboard
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
- **URL**: `https://localhost:8080`
- **Username**: `admin`
- **Password**: Retrieve with `kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`

---

### 6. Kubernetes Dashboard Setup
#### Deploy Dashboard (Minikube)
```bash
minikube dashboard
```
#### For k3s/k8s Cluster:
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
kubectl proxy
```
- **URL**: `http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/`
- **Get Token**:
  ```bash
  kubectl create serviceaccount dashboard-admin -n kubernetes-dashboard
  kubectl create clusterrolebinding dashboard-admin --clusterrole=cluster-admin --serviceaccount=kubernetes-dashboard:dashboard-admin
  kubectl -n kubernetes-dashboard create token dashboard-admin
  ```

---

## 📈 Monitoring & Operations
### Check Pod Status
```bash
kubectl get pods -n voting-app
```

### Check Services
```bash
kubectl get svc -n voting-app
```

### View Logs
```bash
kubectl logs -f <pod-name> -n voting-app
```

### Access the Application
```bash
kubectl port-forward svc/voting-frontend-svc -n voting-app 3000:80
```
- **URL**: `http://localhost:3000`

---

## 🔒 Best Practices
- **GitOps**: ArgoCD syncs manifests directly from Git repo.
- **IaC**: All Kubernetes configs are version-controlled.
- **Monitoring**: Use Kubernetes Dashboard for real-time insights.
- **Security**: Limit secrets exposure, use namespaces for isolation.
- **CI/CD**: (Optional) Integrate with Jenkins/GitHub Actions for automated builds.

---

## 🚨 Troubleshooting
| Issue                  | Solution                                  |
|------------------------|-------------------------------------------|
| Image pull errors      | Check registry auth and image tags in YAML|
| ArgoCD sync failed     | Run `argocd app sync voting-app`          |
| Dashboard inaccessible | Verify service account permissions        |
| Pods in CrashLoop      | Check logs and resource limits in YAML    |

---

## 🌟 Future Enhancements
- Auto-scaling with HPA
- Integrate Prometheus/Grafana for metrics
- Network policies for enhanced security
- Backup/Restore with Velero

---
