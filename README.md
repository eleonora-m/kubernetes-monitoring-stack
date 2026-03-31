# 🎮 Kubernetes Monitored Game Deployment: NodeJS "Battle"

This project showcases a complete DevOps lifecycle: deploying a real NodeJS multiplayer game ("Battle") to a local Kubernetes cluster and implementing a production-grade observability stack using Prometheus and Grafana via Helm.

## 💡 Project Vision
Instead of monitoring an idle cluster, this project deploys a live OpenSource NodeJS application. This allows for end-to-end observability, measuring actual application performance and resource utilization in a realistic scenario.

## 🔄 Architecture Evolution (Cloud to Bare-Metal)
This repository demonstrates infrastructure adaptability, transitioning from managed environments to bare-metal simulation:

* **☁️ Phase 1: Cloud (AWS EKS)**
  * Initial proof-of-concept for AWS Elastic Kubernetes Service.
  * *(Legacy manifests are preserved in the `legacy-eks-manifests/` directory).*

* **🖥️ Phase 2: Bare-Metal / Local Environment (Current)**
  * Local simulation using **Docker Desktop Kubernetes**.
  * **Monitoring Stack:** Utilizes **Helm** for deploying the `kube-prometheus-stack`, aligning with industry standards for complex deployments.
  * **Infrastructure as Code (IaC):** Includes **Ansible** playbooks to demonstrate readiness for bootstrapping real bare-metal nodes (disabling swap, installing containerd, initializing the control plane via `kubeadm`).

## 🛠️ Technologies Used
* **Kubernetes** — Container orchestration
* **NodeJS & Docker** — Application runtime and containerization
* **Prometheus & Grafana** — Metrics scraping and dashboard visualization
* **Helm** — K8s Package management
* **Ansible** — Infrastructure configuration management

## 📂 Project Structure
```text
kubernetes-monitoring-stack/
├── ansible/                  # Ansible playbooks for bare-metal bootstrapping
│   ├── inventory.ini
│   ├── setup-bare-metal.yml  # Prepares all nodes (containerd, swapoff)
│   └── setup-master-node.yml # Initializes control plane (kubeadm init)
├── app-battle-game/          # K8s manifests for the NodeJS game
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   └── secret.yaml           # Note: Ignored in git for security
├── legacy-eks-manifests/     # Reference manifests from Phase 1
└── README.md


🚀 Deployment Instructions
1. Deploy the Target Application

The application is deployed to the default namespace.

Bash
kubectl apply -f app-battle-game/deployment.yaml
kubectl apply -f app-battle-game/service.yaml
kubectl apply -f app-battle-game/configmap.yaml
2. Deploy the Monitoring Stack (Helm)

Ensure Helm is installed, then deploy the Prometheus community stack into a dedicated namespace:

Bash
helm repo add prometheus-community [https://prometheus-community.github.io/helm-charts](https://prometheus-community.github.io/helm-charts)
helm repo update

helm install kube-prometheus-stack prometheus-community/kube-prometheus-stack \
  --namespace monitoring --create-namespace
3. Access the Environments (Port Forwarding)

Since this is a local setup, use port-forwarding to access the services:

Game UI: kubectl port-forward svc/battle-game-service 8080:80
(Access at http://localhost:8080)

Grafana Dashboards: kubectl port-forward svc/kube-prometheus-stack-grafana 3000:80 -n monitoring
(Access at http://localhost:3000)

Prometheus UI: kubectl port-forward svc/kube-prometheus-stack-prometheus 9090:9090 -n monitoring
(Access at http://localhost:9090)

🔔 Key Observability Metrics Captured
Pod CPU/Memory Utilization (Monitored via cAdvisor/kubelet)

Cluster Component Health

Deployment Replica Status

👩‍💻 Author: Eleonora Musaeva | Cloud & DevOps Engineer | GitHub