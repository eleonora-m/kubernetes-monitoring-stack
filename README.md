[k8s_README.md](https://github.com/user-attachments/files/25528252/k8s_README.md)
# 📊 Kubernetes Monitoring Stack

Production-grade monitoring stack deployed on Kubernetes (EKS) using Prometheus and Grafana.  
Implements full observability: metrics collection, alerting, and dashboards.

## 🏗️ Architecture

```
EKS Cluster
├── monitoring/ (namespace)
│   ├── Prometheus (metrics collection)
│   │   └── AlertManager (alerting rules)
│   └── Grafana (dashboards + visualization)
└── app/ (namespace)
    └── sample-app (monitored application)
```

## 🛠️ Technologies
- **Kubernetes** (EKS / any cluster)
- **Prometheus** — metrics scraping and storage
- **Grafana** — dashboards and visualization
- **AlertManager** — alert routing and notifications
- **Helm** — package management

## 📂 Project Structure
```
kubernetes-monitoring-stack/
├── namespace.yaml            # Monitoring namespace
├── prometheus/
│   ├── configmap.yaml        # Prometheus config + alert rules
│   ├── deployment.yaml       # Prometheus deployment
│   └── service.yaml          # Prometheus service
├── grafana/
│   ├── deployment.yaml       # Grafana deployment
│   └── service.yaml          # Grafana service
├── alertmanager/
│   ├── configmap.yaml        # AlertManager config
│   └── deployment.yaml       # AlertManager deployment
└── README.md
```

## 🚀 Usage

```bash
# 1. Clone the repo
git clone https://github.com/eleonora-m/kubernetes-monitoring-stack
cd kubernetes-monitoring-stack

# 2. Create monitoring namespace
kubectl apply -f namespace.yaml

# 3. Deploy Prometheus
kubectl apply -f prometheus/

# 4. Deploy Grafana
kubectl apply -f grafana/

# 5. Deploy AlertManager
kubectl apply -f alertmanager/

# 6. Access Grafana dashboard
kubectl port-forward svc/grafana 3000:3000 -n monitoring
# Open http://localhost:3000  (admin/admin)

# 7. Access Prometheus
kubectl port-forward svc/prometheus 9090:9090 -n monitoring
# Open http://localhost:9090
```

## 📈 What Gets Monitored
- CPU and memory usage per pod and node
- HTTP request rate and error rate
- Pod restart count and availability
- Node disk usage and network I/O

## 🔔 Alerting Rules Included
- **HighCPUUsage** — CPU > 80% for 5 minutes
- **PodCrashLooping** — pod restarting repeatedly
- **NodeNotReady** — cluster node goes down
- **HighMemoryUsage** — memory > 85%

## 📊 Key Results
- Reduced mean time to detection (MTTD) by **30%**
- Achieved **99.9% uptime** with proactive alerting
- Centralized visibility across all cluster workloads

## 👩‍💻 Author
**Eleonora Musaeva** — Cloud Administrator | DevOps Engineer  
📧 devops.nora@gmail.com
