# Kubernetes Course Challenge Labs
---
## 🚀 Introduction
This repository contains solutions to three hands-on lab challenges from my **Viettel High Tech (VHT)** Kubernetes course. The labs focus on deploying, configuring, and managing containerized applications using Kubernetes resources like Deployments and Services. Each challenge includes detailed instructions, commands used, explanations of chosen methods, and screenshots of the results.

## 🔧 Prerequisites
Ensure you have the following tools installed:
- [Docker](https://www.docker.com/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/)
- A web browser for testing deployed applications
> **📝 Note**: Make sure you have `minikube` running before deploying any services. Use the command:
> ```bash
> minikube start
> ```
---

## 🛠️ Lab Challenges Overview
> **📝 Note**: This project was developed and tested on a **macOS** environment. Due to macOS network limitations with `minikube`, I used minikube tunnel to access the services deployed using **NodePort**:
> ```
> minikube service <service-name> --url
> ```
>  This is necessary because the default method of accessing services via NodePort does not work seamlessly on macOS. Refer to these links for more details.
> (https://minikube.sigs.k8s.io/docs/handbook/accessing/), (https://github.com/kubernetes/minikube/issues/11193)


### Challenge 1: Default nginx - Basic
**Objective**: Deploy a static website using NGINX in a Kubernetes cluster and expose it via a NodePort service.

**Detail***(In Vietnamese)*:

> Đề bài: triển khai deployment chạy nginx (default) lên kubernetes và cho phép truy cập từ bên ngoài thông qua NodePort
> Output:
> - 1 deployment nginx (replicas=2 pod)
> - 1 nodePort service trỏ tới deployment
> - thực hiên curl tới nodePort và cho ra kết quả trang web mặc định của nginx

#### 📝 Step-by-step Process

   - Created `nginx-default.yaml` file for the NGINX static website.
   - Applied the deployment and service:
     ```bash
     kubectl apply -f nginx-default.yaml
     ```

     Or using imperative approach by exposing the deployment using a NodePort service:
     ```bash
     kubectl expose deployment nginx-default --type=NodePort --port=80
     ```
  - Check the state of deployment and service:
    ```
    kubectl get all -l app-nginx
    kubectl get svc nginx-default
    ```
    
   - Accessed the website:
     ```bash
     minikube service nginx-default
     ```



---

### Challenge 2: NGINX Proxy for Multiple Applications
**Objective**: Set up an NGINX proxy to forward traffic to two separate static websites deployed in the cluster.

#### 📝 Step-by-step Process
1. **Deploying Websites**
   - Built Docker images for two static websites (`web1` and `web2`).
     ```bash
     docker build -t myusername/web1:latest -f Dockerfile.web1 .
     docker build -t myusername/web2:latest -f Dockerfile.web2 .
     ```
   - Created deployments and services:
     ```bash
     kubectl apply -f web1-deployment.yaml
     kubectl apply -f web2-deployment.yaml
     ```

2. **Setting up NGINX Proxy**
   - Configured `nginx.conf` to route `/web1` to `web1` service and `/web2` to `web2` service.
   - Built the NGINX proxy Docker image:
     ```bash
     docker build -t myusername/nginx-proxy:latest -f Dockerfile.proxy .
     ```
   - Created deployment and NodePort service for the proxy:
     ```bash
     kubectl apply -f nginx-proxy-deployment.yaml
     kubectl apply -f nginx-proxy-service.yaml
     ```
   - Accessed the proxy using `minikube service nginx-proxy`.

#### 📸 Screenshots
- Upload your screenshots here showing the proxy forwarding to both websites.

---

### Challenge 3: Exposing Applications via NodePort & ClusterIP
**Objective**: Understand the differences between ClusterIP and NodePort services by exposing microservices.

#### 📝 Step-by-step Process
1. **Deploying Microservices**
   - Built Docker images for microservices:
     ```bash
     docker build -t myusername/service1:latest -f Dockerfile.service1 .
     docker build -t myusername/service2:latest -f Dockerfile.service2 .
     ```
   - Created deployments and services:
     ```bash
     kubectl apply -f service1-deployment.yaml
     kubectl apply -f service2-deployment.yaml
     ```

2. **Service Exposure**
   - Exposed `service1` using NodePort:
     ```bash
     kubectl expose deployment service1 --type=NodePort --port=80
     ```
   - Exposed `service2` using ClusterIP and tested with `kubectl port-forward`:
     ```bash
     kubectl port-forward svc/service2 8080:80
     ```

3. **Testing Services**
   - Verified the NodePort service using:
     ```bash
     minikube service service1
     ```

#### 📸 Screenshots
- Upload your screenshots here showing the microservices working correctly.

---

## 🏁 Getting Started

### Clone the Repository
```bash
git clone https://github.com/yourusername/kubernetes-challenge-labs.git
cd kubernetes-challenge-labs
