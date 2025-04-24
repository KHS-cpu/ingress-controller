# Ingress-controller
This project demonstrates a simple Kubernetes Ingress setup using NGINX Ingress Controller. It exposes two services — `wear` and `watch` — via path-based routing using a LoadBalancer.

---
## 📦 Architecture Diagram

![Architecture](https://github.com/KHS-cpu/ingress-controller/blob/main/ingress%20controller.png)

---

## 📦 Architecture Overview

Instead of using three separate containers for HTML, Wear, and Watch, this setup uses single pod:
- `wear` serves content at `/wear`
- `watch` serves content at `/watch`
- `html` serves content at `/html`

All traffic is routed through the Ingress controller.

---

## ⚙️ Components

- **Ingress Controller (NGINX)**: Handles incoming HTTP requests and routes them based on path.
- **ClusterIP Service**: Exposes the service using ClusterIP to outside.
  - Ingress is defined to handle requests for the host www.khs-lab:, and with path for example http://www.khs-lab/wear , http://www.khs-lab/watch etc.
- **Deployments**:
  - `wear`: serves content at `http://khs-lab/wear`
  - `watch`: serves content at `http://khs-lab/watch`
  - `html`: serves content at `http://khs-lab/html`

---

## 🌐 Accessing the Services

Once deployed on a Kubernetes cluster:

- `http://khs-lab/wear` → routed to the `wear` path
- `http://khs-lab/watch` → routed to the `watch` path
- `http://khs-lab/html` → routed to the `html` path

---

## What is Ingress Controller
An Ingress Controller in Kubernetes is used to route external traffic to multiple services running inside the cluster. Instead of exposing each service with a separate LoadBalancer, the Ingress Controller acts as a reverse proxy, allowing you to define rules for routing traffic based on path or hostname. This reduces the need for multiple external IPs and helps you expose only the necessary services to the public while keeping others internal.

---

## Install ingress controller with helm
- **helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx**
- **helm repo update**
- **helm install nginx-ingress ingress-nginx/ingress-nginx --namespace default**

- **Install configmap.yaml & nginx-config.yaml first then install deployment.yaml & service.yaml**
- **Install ingress-resource.yaml file to put ingress-resource to ingress controller.**
- **Check the ingress controller get external IP by using**
- **kubectl get svc**

## Apply that IP to host /etc/hosts path
- **echo "172.18.255.151 www.khs-lab" | sudo tee -a /etc/hosts > /dev/null  # Since this is lab environment I use this as local DNS**

## Test if it is working or not
- **curl http://www.khs-lab/html**

## You can also test if it is working in browser also

---

## 🚧 Notes

- This setup uses path-based routing via Ingress. In production, separating each service into its own deployment with unique routes or subdomains is preferable.
- Due to time constraints, HTML content is embedded within the existing `watch` and `wear` pods.

## 🏗️ Future Improvements

- Separate the three services with three different deployment.
- Test this on AWS EKS using alb ingress controller exposing as Load Balancer.
- Add SSL for more secure.
- Reintegrate with Terraform code for IaC and automation.
- Integrate CI/CD for easier updates.
