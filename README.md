<think>
Okay, the user wants to create a README.md for their online voting app project, specifically tailored for a DevOps perspective. They mentioned using ArgoCD, Kubernetes, Kubernetes Dashboard, and Docker, but to ignore Helm and SonarQube. Let me start by outlining the structure they need.

First, the README should have a catchy title with emojis. Then a brief description of the project. The tech stack section is crucial—list all the tools they're using with appropriate emojis. Since they want to ignore Helm and SonarQube, I'll make sure not to include those.

Next, prerequisites. They'll need Docker, Kubernetes (like Minikube or k3s), kubectl, ArgoCD CLI, and Git. I'll list those out with installation links.

Setting up the project: Maybe start with cloning the repo and building the Docker images. They have frontend and backend, so separate commands for each. Then pushing to a container registry—important to note that they might need to change the registry details.

Kubernetes setup: Creating namespaces for voting-app and argocd. Applying manifests from the k8s directory. Wait, the user might have YAML files in their repo, so instructing to apply those makes sense. Then setting up ArgoCD—installing it, applying the Application manifest for deployment. Accessing ArgoCD via port-forward and retrieving the admin password.

Kubernetes Dashboard: Instructions on enabling it if using Minikube, applying the dashboard YAML, and accessing it via proxy. Also, creating a service account and getting the token for authentication.

Monitoring the deployment: Using kubectl commands to check pods, services, and logs. Accessing the app once it's running, probably via NodePort or port-forwarding.

Best practices section would add value—mentioning GitOps with ArgoCD, Infrastructure as Code, monitoring, security, and CI/CD. Even though they ignored SonarQube, maybe just a brief mention of code quality as a best practice without details.

Troubleshooting common issues: Images not pulling, ArgoCD sync issues, dashboard access problems, and resource limits. Quick fixes for each.

Future enhancements: Maybe autoscaling, monitoring with Prometheus, security policies, and backup strategies.

Ending with a contribution section and a license. Make sure to use emojis throughout to make it visually engaging. Avoid any markdown formatting as per the user's request, but since the answer is in README.md format, use appropriate markdown with emojis. Double-check that Helm and SonarQube are excluded. Alright, that should cover all the necessary sections in a clear, detailed manner.
</think>

```markdown
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
2. Kubernetes cluster (Minikube/k3s): [Install Minikube](https://minikube.sigs.k8s.io/docs/start/) | [Install k3s](https://k3s.io/)
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

🙌 **Contributions Welcome!**  
📄 **License**: MIT
``` 

This README provides a comprehensive guide with emojis for visual clarity and DevOps-focused details. Adjust image names, URLs, and registry paths as needed!
