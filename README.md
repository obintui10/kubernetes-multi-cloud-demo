# Kubernetes Multi‑Cloud Demo

[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.29-blue?logo=kubernetes)](https://kubernetes.io/)
[![AWS EKS](https://img.shields.io/badge/AWS-EKS-orange?logo=amazon-aws)](https://aws.amazon.com/eks/)
[![Azure AKS](https://img.shields.io/badge/Azure-AKS-blue?logo=microsoft-azure)](https://azure.microsoft.com/en-us/services/kubernetes-service/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A hands‑on demonstration of deploying workloads across **AWS EKS** and **Azure AKS** using Kubernetes manifests.  
This repo showcases **shared Kubernetes fundamentals** (Deployment, ConfigMap) alongside **cloud‑specific orchestration** (Service, Ingress with provider annotations).

## 📂 Repository Structure
.
├── LICENSE
├── README.md
└── manifests/
    ├── shared/                   # Portable, cloud-agnostic components
    │   ├── deployment.yaml       # Defines 3 replicas of nginx:latest
    │   └── configmap.yaml        # APP_ENV and APP_DEBUG variables
    ├── aws/                      # EKS-specific infrastructure
    │   ├── service.yaml          # NLB annotations (service.beta.k8s.io/aws-load-balancer-type)
    │   └── ingress.yaml          # ALB Ingress Controller configurations
    └── azure/                    # AKS-specific infrastructure
        ├── service.yaml          # Internal LB annotations (service.beta.k8s.io/azure-load-balancer-internal)
        └── ingress.yaml          # Azure Application Gateway (AGIC) rules


## 🚀 Quickstart

1. **Clone the repo**
   ```bash
   git clone https://github.com/obintui10/kubernetes-multi-cloud-demo.git
   cd kubernetes-multi-cloud-demo

## Apply shared manifests
- kubectl apply -f manifests/shared/deployment.yaml
- kubectl apply -f manifests/shared/configmap.yaml

## Apply cloud‑specific manifests
## For AWS EKS:
- kubectl apply -f manifests/aws/service.yaml
- kubectl apply -f manifests/aws/ingress.yaml

## For Azure AKS:
- kubectl apply -f manifests/azure/service.yaml
- kubectl apply -f manifests/azure/ingress.yaml

## 🔄 Workflow
- Deployment → Creates 3 replicas of nginx:latest.
- ConfigMap → Injects environment variables (APP_ENV, APP_DEBUG).
- Service → Exposes pods via LoadBalancer (AWS NLB / Azure Internal LB).
- Ingress → Routes external traffic via ALB (AWS) or App Gateway (Azure).

## 🏗 Architecture Diagram

```text
+---------------------------------------------------+
|                 Pods (demo-app)                   |
|   - 3 replicas of nginx:latest                    |
|   - Uses ConfigMap for APP_ENV + APP_DEBUG        |
+---------------------------------------------------+
                          |
                          v
+---------------------------------------------------+
|                Service (LoadBalancer)             |
|   - Exposes pods on port 80                       |
|   - Type: LoadBalancer                            |
+---------------------------------------------------+
                          |
                          v
+---------------------------------------------------+
|                     Ingress                       |
|   - Routes external traffic                       |
|   - Host-based rules (demo.example.com)           |
+---------------------------------------------------+
             /                               \
            v                                 v
+---------------------------+     +-----------------------------+
|        AWS (EKS)          |     |        Azure (AKS)          |
|  - NLB (Network LB)       |     |  - Internal Load Balancer   |
|  - ALB (Ingress Controller)|     |  - Application Gateway      |
|  - Annotations:            |     |  - Annotations:             |
|    service.beta.k8s.io/... |     |    service.beta.k8s.io/...  |
+---------------------------+     +-----------------------------+


## 🛠 Tech Stack
- Kubernetes (apps/v1, networking.k8s.io/v1)
- AWS EKS (Network Load Balancer, ALB Ingress Controller)
- Azure AKS (Internal Load Balancer, Application Gateway Ingress)
- Helm/Terraform (optional extensions for infra provisioning)

## 📌 Future Work
- Add Helm charts for packaging workloads.
- Extend Terraform integration for cluster provisioning.
- Demonstrate CI/CD pipeline deploying manifests to EKS + AKS.
- Add monitoring stack (Prometheus + Grafana).
