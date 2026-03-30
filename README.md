```markdown
# 🎮 Kubernetes Monitored Game Deployment: NodeJS "Battle"

This project showcases a complete DevOps lifecycle: deploying a real NodeJS multiplayer game ("Battle") to a local Kubernetes cluster and implementing a production-grade monitoring stack using Prometheus and Grafana.

## 💡 Project Vision
Instead of monitoring an idle cluster, this project deploys a live OpenSource NodeJS application. This allows for end-to-end observability, measuring actual application performance, request rates, and resource utilization in a realistic scenario.

## 🔄 Architecture Evolution (Cloud to Bare-Metal)
This repository demonstrates infrastructure adaptability, specifically transitioning from a managed cloud environment to an on-premise (bare-metal) simulation:

* **☁️ Phase 1: Cloud (AWS EKS)**
  * Initial proof-of-concept deployed on AWS Elastic Kubernetes Service.
  * Kubernetes resources (Deployments, Services, ConfigMaps) were managed via manual YAML manifests.
  * *(These legacy manifests are preserved in the `legacy-eks-manifests/` directory for reference).*

* **🖥️ Phase 2: Bare-Metal / Local Environment (Current)**
  * Rebuilt locally using **Docker Desktop Kubernetes** to simulate an on-premise, air-gapped-friendly infrastructure.
  * **Target Workload:** The NodeJS "Battle" game containerized and deployed.
  * **Monitoring Stack:** Transitioned to **Helm** charts for Prometheus and Grafana, aligning with industry standards for managing complex workloads on self-hosted servers.
  * **Automation:** Added basic **Ansible** playbooks to demonstrate configuration management readiness for bare-metal nodes.

## 🛠️ Technologies Used
* **Kubernetes** (AWS EKS & Docker Desktop)
* **NodeJS & Docker** — Application runtime and containerization
* **Prometheus** — Metrics scraping and time-series storage
* **Grafana** — Dashboards and visualization
* **Helm** — Package management for K8s
* **Ansible** — Infrastructure automation

## 📂 Project Structure
```text
kubernetes-monitoring-stack/
├── app-battle-game/          # K8s manifests for the NodeJS application
│   ├── deployment.yaml
│   └── service.yaml
├── monitoring-helm/          # Helm values and custom Grafana dashboards
│   └── grafana-dashboard.json 
├── legacy-eks-manifests/     # Original manual YAML manifests from Phase 1
│   ├── prometheus/
│   ├── grafana/
│   └── alertmanager/
├── ansible/                  # Automation playbooks
│   └── setup-nodes.yaml 
└── README.md
```

## 🚀 Deployment Instructions

### 1. Deploy the Target Application ("Battle" Game)
```bash
kubectl create namespace game-ns
kubectl apply -f app-battle-game/ -n game-ns
```

### 2. Deploy the Monitoring Stack (Helm)
Ensure Helm is installed, then deploy the Prometheus community stack:
```bash
helm repo add prometheus-community [https://prometheus-community.github.io/helm-charts](https://prometheus-community.github.io/helm-charts)
helm repo update

helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring --create-namespace
```

### 3. Access the Environments
* **Game:** `kubectl port-forward svc/battle-game-service 8080:80 -n game-ns`
* **Grafana:** `kubectl port-forward svc/monitoring-grafana 3000:80 -n monitoring`

## 🔔 Key Observability Metrics Captured
* **Node Availability & Pod Uptime**
* **CPU/Memory Utilization** (Impact of the game load on the cluster)
* **HTTP Request Rates & Error Rates** (Application stability)

---
**👩‍💻 Author:** Eleonora Musaeva | Cloud & DevOps Engineer | [GitHub](https://github.com/eleonora-m)
```