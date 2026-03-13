# 🚀 Kubernetes Deployment Manifests – Todo App (GitOps)

This repository contains the **Kubernetes manifests used to deploy the Todo App container into a Kubernetes cluster**.

It is part of a **GitOps workflow**, where updates to Kubernetes configuration are automatically pushed by a CI/CD pipeline.

Think of this repo as the **instruction manual for Kubernetes** 📘.

Instead of telling Kubernetes manually what to do, we store the instructions here and Kubernetes follows them.

---

# 🧭 Architecture Overview

Below is the high-level flow of how the application reaches users.

```
Developer Push Code
        │
        ▼
   CI/CD Pipeline
   (CircleCI)
        │
        ▼
Docker Image Built
        │
        ▼
  Docker Hub Registry
        │
        ▼
GitOps Repo Updated
(This Repository)
        │
        ▼
Kubernetes Cluster
        │
        ▼
 Deployment → Pods → Service → Users
```

In simple terms:

Code → Docker Image → Kubernetes Deployment → Service → Users

---

# 📂 Repository Structure

```
.
└── manifest
    ├── deployment.yml
    └── service.yml
```

| File | Description |
|-----|------|
| deployment.yml | Defines how the application containers should run |
| service.yml | Exposes the application to the network |

---

# ⚙️ deployment.yml Explained

This file defines **how Kubernetes should run the application**.

Full configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp

spec:
  selector:
    matchLabels:
      app: myapp

  replicas: 2

  template:
    metadata:
      labels:
        app: myapp

    spec:
      containers:
       - name: myapp
         image: avulamahesh/todo-app:build-42
         ports:
         - containerPort: 80
```

---

# 🔹 apiVersion

```
apiVersion: apps/v1
```

This tells Kubernetes which API version is used to create the deployment.

Most modern deployments use:

```
apps/v1
```

---

# 🔹 kind

```
kind: Deployment
```

This specifies the Kubernetes resource type.

A **Deployment** manages application pods and provides:

• Automatic scaling  
• Rolling updates  
• Self-healing  

If a pod crashes, Kubernetes automatically recreates it.

---

# 🔹 metadata

```
metadata:
  name: myapp
```

This gives a name to the deployment.

Check deployments with:

```
kubectl get deployments
```

---

# 🔹 selector

```
selector:
  matchLabels:
    app: myapp
```

This tells Kubernetes which pods belong to this deployment.

Pods are identified using **labels**.

Labels act like **ID cards for Kubernetes objects**.

---

# 🔹 replicas

```
replicas: 2
```

This tells Kubernetes to maintain **2 running pods**.

Why?

If one pod crashes, the other continues serving users.

Example:

```
Pod 1 → Running
Pod 2 → Running
```

This improves **availability and reliability**.

---

# 🔹 template

```
template:
```

This section defines **how each pod should be created**.

It contains:

• Pod labels  
• Container specifications  
• Images  
• Ports  

---

# 🔹 container configuration

```
containers:
 - name: myapp
```

Defines the container running inside the pod.

---

# 🔹 Docker Image

```
image: avulamahesh/todo-app:build-42
```

This tells Kubernetes which Docker image to run.

Image format:

```
<dockerhub-username>/<image-name>:<tag>
```

Example:

```
avulamahesh/todo-app:build-42
```

This image is automatically built and pushed by the **CI/CD pipeline**.

---

# 🔹 containerPort

```
containerPort: 80
```

This defines the port where the application listens inside the container.

---

# 🌐 service.yml Explained

Pods inside Kubernetes are **not directly accessible externally**.

A **Service** provides network access.

Full configuration:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service

spec:
  selector:
    app: myapp
    
  type: LoadBalancer

  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
```

---

# 🔹 apiVersion

```
apiVersion: v1
```

Services use the Kubernetes **core API version**.

---

# 🔹 kind

```
kind: Service
```

This defines a **Service resource**.

A Service routes network traffic to pods.

---

# 🔹 metadata

```
metadata:
  name: myapp-service
```

Name of the service.

View services with:

```
kubectl get svc
```

---

# 🔹 selector

```
selector:
  app: myapp
```

This connects the service to the pods.

The service forwards traffic to pods with label:

```
app=myapp
```

---

# 🔹 type

```
type: LoadBalancer
```

This exposes the application to the **internet**.

Cloud providers automatically create a load balancer.

Examples:

AWS → Elastic Load Balancer  
Azure → Azure Load Balancer  
GCP → Cloud Load Balancer  

---

# 🔹 ports

```
ports:
 - port: 80
   protocol: TCP
   targetPort: 80
```

Explanation:

| Field | Meaning |
|-----|-----|
| port | Port exposed by service |
| targetPort | Port inside container |
| protocol | Network protocol |

Traffic flow:

```
User
 ↓
Cloud LoadBalancer
 ↓
Kubernetes Service
 ↓
Pod
 ↓
Container
```

---

# 🚀 Deployment Steps

Deploy application

```
kubectl apply -f deployment.yml
```

Expose service

```
kubectl apply -f service.yml
```

Check pods

```
kubectl get pods
```

Check services

```
kubectl get svc
```

---

# 🔄 GitOps Workflow

This repository is updated automatically by a **CI/CD pipeline**.

Pipeline process:

```
Code Change
   ↓
CircleCI Pipeline
   ↓
Docker Image Built
   ↓
Image Pushed to Docker Hub
   ↓
Deployment YAML Updated
   ↓
Git Commit to Manifest Repo
   ↓
Kubernetes Deploys New Version
```

This ensures **fully automated deployments**.

---

# 🧠 Concepts Demonstrated

This repository demonstrates:

✔ Kubernetes Deployments  
✔ Pod Scaling with Replicas  
✔ Container Deployment using Docker Images  
✔ Kubernetes Service Networking  
✔ LoadBalancer for external access  
✔ GitOps based deployment automation

---

# 👨‍💻 Author

Mahesh Avula  
Cloud / DevOps Engineer

Interested in building **cloud-native systems, Kubernetes platforms, and automated CI/CD pipelines** ☁️⚙️