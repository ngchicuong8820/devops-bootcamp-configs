
# 🚀 DevOps Bootcamp Capstone Project

[![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-red?logo=jenkins)](http://32.195.232.187:9080)
[![Docker](https://img.shields.io/badge/Docker-Container-blue?logo=docker)](https://hub.docker.com/u/ngchicuong8820)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestration-blue?logo=kubernetes)](https://kubernetes.io)
[![Grafana](https://img.shields.io/badge/Grafana-Monitoring-orange?logo=grafana)](http://32.195.232.187:3001)
[![Prometheus](https://img.shields.io/badge/Prometheus-Metrics-red?logo=prometheus)](http://32.195.232.187:9090)

## 📋 Project Overview

A production-grade DevOps implementation of a **TodoList Application** built with **Next.js** (Frontend) and **Express.js** (Backend), deployed on **AWS EC2** with full CI/CD automation, container orchestration, and observability stack.

---

## 🏗️ Architecture Overview
```mermaid
flowchart TD
                    ┌──────────────────────────────────────┐
                    │           GitHub Repositories        │
                    │  frontend-repo  │  backend-repo      │
                    └────────┬────────┴────────┬───────────┘
                             │   Webhook        │
                             ▼                  ▼
                    ┌─────────────────────────────────────┐
                    │          Jenkins CI/CD               │
                    │         (port 9080)                  │
                    │                                      │
                    │  Source → Build → Test+Lint+DepCheck │
                    │  → ImageScan → Push → Deploy Dev     │
                    │  → Approval → Deploy Prod            │
                    └──────────┬──────────────────────────┘
                               │
           ┌───────────────────┼───────────────────┐
           ▼                   ▼                   ▼
┌──────────────────┐  ┌───────────────┐  ┌────────────────┐
│   Dev Environment│  │  K8s (Kind)   │  │   Monitoring   │
│  Docker Compose  │  │  Production   │  │    Stack       │
│                  │  │               │  │                │
│ todolist-backend │  │ backend x3    │  │ Prometheus     │
│ todolist-frontend│  │ frontend x2   │  │ Grafana        │
└────────┬─────────┘  └──────┬────────┘  │ Loki+Promtail  │
         │                   │           └────────────────┘
         └─────────┬─────────┘
                   ▼
        ┌──────────────────────┐
        │      DB EC2          │
        │  PostgreSQL          │
        │  todolist_dev        │
        │  todolist_prod       │
        └──────────────────────┘
---

# ## 🔗 Repositories

$$
| Repository | Description | Link |
|------------|-------------|------|
| **devops-bootcamp-configs** | DevOps configurations (this repo) | [Link](https://github.com/ngchicuong8820/devops-bootcamp-configs) |
| **todolist-backend-api** | Express.js Backend API | [Link](https://github.com/ngchicuong8820/devops-bootcamp-todolist-backend-api) |
| **todolist-frontend** | Next.js Frontend | [Link](https://github.com/ngchicuong8820/devops-bootcamp-todolist-frontend) |
$$

---

# ## 🌐 Demo URLs
| Service | URL | Description |
|---------|-----|-------------|
| 🖥️ Frontend | [http://32.195.232.187:3000](http://32.195.232.187:3000) | TodoList Web App |
| ⚙️ Backend API | [http://32.195.232.187:5000](http://32.195.232.187:5000) | REST API |
| 🔧 Jenkins | [http://32.195.232.187:9080](http://32.195.232.187:9080) | CI/CD Dashboard |
| 📊 Grafana | [http://32.195.232.187:3001](http://32.195.232.187:3001) | Monitoring Dashboard |
| 📈 Prometheus | [http://32.195.232.187:9090](http://32.195.232.187:9090) | Metrics |

---

# ## 🛠️ Tech Stack
| Category | Technology | Purpose |
|----------|-----------|---------|
| **Cloud** | AWS EC2 | Virtual Machines |
| **Container** | Docker, Docker Compose | Containerization |
| **Orchestration** | Kubernetes (Kind) | Production deployment |
| **CI/CD** | Jenkins | Automation pipeline |
| **Monitoring** | Prometheus | Metrics collection |
| **Visualization** | Grafana | Dashboard & Alerting |
| **Log Management** | Loki + Promtail | Log aggregation |
| **Database** | PostgreSQL | Data persistence |
| **Registry** | Docker Hub | Image registry |
| **Security Scan** | Trivy | Container vulnerability scan |
| **Dependency Check** | npm audit | Package vulnerability check |

---


# ## 📁 Repository Structure
```mermaid
flowchart TD
devops-bootcamp-configs/
├── 📁 docker/
│   └── docker-compose.dev.yml        # Dev environment
├── 📁 k8s/
│   ├── namespace.yaml                # K8s namespace
│   ├── ingress.yaml                  # NGINX Ingress
│   ├── 📁 backend/
│   │   ├── deployment.yaml           # Backend deployment (3 replicas)
│   │   ├── service.yaml              # Backend service
│   │   └── configmap.yaml            # Backend config
│   ├── 📁 frontend/
│   │   ├── deployment.yaml           # Frontend deployment (2 replicas)
│   │   ├── service.yaml              # Frontend service
│   │   └── configmap.yaml            # Frontend config
│   └── 📁 postgres/
│       ├── secret.yaml               # DB credentials (encrypted)
│       └── service.yaml              # DB service
├── 📁 monitoring/
│   ├── 📁 prometheus/
│   │   └── prometheus.yml            # Prometheus scrape config
│   ├── 📁 loki/
│   │   └── loki-config.yml           # Loki configuration
│   ├── 📁 promtail/
│   │   └── promtail-config.yml       # Promtail configuration
│   └── 📁 grafana-dashboards/
│       ├── infrastructure-dashboard.json
│       └── application-dashboard.json
├── docker-compose.monitoring.yml     # Monitoring stack
└── README.md
---

## 🔄 CI/CD Pipeline
```mermaid
flowchart TD

** Pipeline Stages**
─────────┐   ┌───────┐   ┌──────────────────────┐
│ Source  │──▶│ Build │──▶│ Test & Quality Gate   │
└─────────┘   └───────┘   │ (Parallel)            │
│ ├── Unit Test         │
│ ├── Linter            │
│ └── Dependency Check  │
└──────────┬────────────┘
│
┌──────────▼────────────┐
│   Push to Docker Hub  │
└──────────┬────────────┘
│
┌──────────▼────────────┐
│   Image Scan (Trivy)  │
└──────────┬────────────┘
│
┌──────────▼────────────┐
│   Deploy Dev          │
│   (Docker Compose)    │
└──────────┬────────────┘
│
┌──────────▼────────────┐
│   Manual Approval     │
└──────────┬────────────┘
│
┌──────────▼────────────┐
│   Deploy Production   │
│   (Kubernetes)        │
└───────────────────────┘

### Key Features
- ✅ **Webhook trigger**: Auto-build on push/PR
- ✅ **Parallel stages**: Test + Lint + Dependency Check
- ✅ **DevSecOps**: npm audit + Trivy image scan
- ✅ **Secret management**: Jenkins Credentials Store
- ✅ **Manual approval**: Gate before production
- ✅ **Rolling update**: Zero-downtime deployment

---

## 📊 Monitoring & Observability

### Prometheus Targets (6 targets)

| Target | Port | Description |
|--------|------|-------------|
| prometheus | 9090 | Self-monitoring |
| cadvisor | 8080 | Container metrics |
| node-exporter | 9100 | EC2 Medium host metrics |
| grafana | 3000 | Grafana metrics |
| todolist-backend | 5000 | API metrics (/metrics) |
| db-ec2-node | 9100 | DB EC2 host metrics |

### Grafana Dashboards

| Dashboard | Description |
|-----------|-------------|
| **Infrastructure** | CPU, Memory, Disk, Network for 2 EC2s |
| **Application** | Container metrics, API performance, Logs |

### Alert Rules

| Alert | Source | Threshold |
|-------|--------|-----------|
| High CPU Usage | Prometheus | > 80% for 5min |
| High Memory Usage | Prometheus | > 85% for 5min |
| High Disk Usage | Prometheus | > 90% for 1min |
| Backend Service Down | Prometheus | up < 1 for 1min |
| Error Logs Detected | Loki | error count > 0 |

---

## 🔒 Security

### Implementation
- **SSH**: Key pair authentication (no password)
- **Credentials**: Jenkins Credentials Store + Kubernetes Secrets
- **Security Groups**: Separated by responsibility
  - `capstone-sg-tool`: Public traffic (App + SSH)
  - `sg-monitoring`: Monitoring tools access
  - `database-sg`: DB access (EC2 Medium private IP only)
- **DevSecOps Pipeline**:
  - npm audit: Dependency vulnerability check
  - Trivy: Container image vulnerability scan
- **Database**: Private IP only, no public exposure

---

## ⚙️ Infrastructure

### EC2 Instances

| Instance | Type | Purpose |
|----------|------|---------|
| EC2 Medium | t3.medium | App + Tools + K8s |
| DB EC2 | t3.micro | PostgreSQL Database |

### Database

| Database | Environment | Used By |
|----------|-------------|---------|
| todolist_dev | Development | Docker Compose |
| todolist_prod | Production | Kubernetes |

---

## ⚠️ Known Limitations & Trade-offs

| Limitation | Reason | Production Solution |
|------------|--------|---------------------|
| Default VPC | AWS Academy credit limit | Custom VPC with public/private subnets |
| 2 EC2 instead of 3 | Credit optimization | Separate App, Tools, DB instances |
| Docker Hub vs ECR | Free tier | AWS ECR with IAM policies |
| Monitoring tools exposed | Dynamic IP issue | VPN or Bastion host |
| No HTTPS | No domain/certificate | AWS ACM + ALB |
| No NAT Gateway | Cost ~$1/day | NAT Gateway per AZ |

---

## 👤 Author

Nguyen Chi Cuong
- GitHub: [@ngchicuong8820](https://github.com/ngchicuong8820)
- Bootcamp: VNTechies DevOps Bootcamp
