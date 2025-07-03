```markdown
# Advanced CI/CD Demo with ArgoCD & Argo Rollouts
```
This project demonstrates a **GitOps-based deployment pipeline** using **Argo CD**, **Argo Rollouts**, and **Kubernetes** for a sample Nginx web application. It features **progressive delivery** using Canary deployments, automatic rollbacks, and live monitoring.

---

## Project Structure



advanced-cicd-demo/
├── app/
│   ├── Dockerfile         # Dockerfile for Nginx web app
│   └── index.html         # Simple HTML page
├── manifests/
│   ├── deployment.yaml    # Kubernetes Rollout manifest
│   ├── service.yaml       # Kubernetes Service manifest
│   └── analysis.yaml      # Argo Rollouts AnalysisTemplate for health checks
└── README.md              # This file



##  Features

 **GitOps workflow with ArgoCD**  
 **Canary deployments with Argo Rollouts**  
 **Automatic health checks using Prometheus**  
 **Safe rollbacks on deployment failure**  
 **Visual dashboards for deployment monitoring**

---

##  Tech Stack

| Tool               | Purpose                                  |
|--------------------|------------------------------------------|
| **ArgoCD**         | GitOps-based Continuous Delivery         |
| **Argo Rollouts**  | Canary deployments & progressive delivery|
| **Docker**         | Containerizing the Nginx web app         |
| **Kubernetes**     | Container orchestration                  |
| **Prometheus**     | Metrics collection for health checks     |
| **GitHub**         | Version control & single source of truth |

---

##  Quick Start

### 🖥 Prerequisites
- Docker
- kubectl
- minikube/kind (or any K8s cluster)
- Argo CD & Argo Rollouts installed in the cluster
- Access to Docker Hub (for image pushes)

---

###  Setup Instructions

#### 1️⃣ Clone the Repository
```bash
git clone https://github.com/<your-username>/advanced-cicd-demo.git
cd advanced-cicd-demo
````

#### 2️⃣ Build & Push Docker Image

```bash
cd app
docker build -t <your-dockerhub-username>/webapp:v1 .
docker push <your-dockerhub-username>/webapp:v1
```

#### 3️⃣ Update Deployment Image

Edit `manifests/deployment.yaml`:

```yaml
containers:
  - name: webapp
    image: <your-dockerhub-username>/webapp:v1
```

---

#### 4️⃣ Deploy with Argo CD

```bash
kubectl apply -f manifests/
```

Access Argo CD UI:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Visit [http://localhost:8080](http://localhost:8080) and login.

---

#### 5️⃣ Monitor Rollouts

Port-forward Argo Rollouts Dashboard:

```bash
kubectl argo rollouts dashboard
```

Or watch rollout in terminal:

```bash
kubectl argo rollouts get rollout webapp-rollout --watch
```

---

#### 6️⃣ Access the App

Port-forward the service:

```bash
kubectl port-forward svc/webapp-service 8080:80
```

Open in browser: [http://localhost:8080](http://localhost:8080)

---

## Demo: Progressive Delivery

1. Edit `app/index.html`:

```html
<h1>Hello from v2.0 🎉</h1>
```

2. Build & Push:

```bash
docker build -t <your-dockerhub-username>/webapp:v2 .
docker push <your-dockerhub-username>/webapp:v2
```

3. Update `manifests/deployment.yaml` with `v2`
4. Commit & Push:

```bash
git add .
git commit -m "Deploy v2.0"
git push origin main
```

5. Watch Canary deployment in Rollouts dashboard or CLI.

---

## Dashboards

| Dashboard    | URL                                            |
| ------------ | ---------------------------------------------- |
| **Argo CD**  | [http://localhost:8080](http://localhost:8080) |
| **Rollouts** | [http://localhost:3100](http://localhost:3100) |

---

## Authors

*  Akshay Malik
*  Team Members
---

## Learn More

* [Argo CD Docs](https://argo-cd.readthedocs.io)
* [Argo Rollouts Docs](https://argoproj.github.io/argo-rollouts/)
* [Kubernetes Docs](https://kubernetes.io/docs/)

```
```
