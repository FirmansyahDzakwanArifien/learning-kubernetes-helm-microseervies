Microservices K8s Helm Tutorial

A comprehensive hands-on tutorial for deploying microservices applications, demonstrating the evolution from conventional Kubernetes YAML manifests to modern Helm Charts implementation using Google's Hipster Shop microservices demo.
🎯 Learning Objectives
By completing this tutorial, you will learn to:

Deploy microservices using native Kubernetes YAML manifests
Understand the challenges of managing multiple microservices configurations
Create reusable Helm Charts for microservices applications
Implement template-based deployment strategies
Use Helmfile for managing multiple Helm releases
Apply DevOps best practices for containerized applications
Troubleshoot common deployment issues across different platforms

🏗️ Architecture Overview
This tutorial uses Google's Hipster Shop - a cloud-native microservices demo application consisting of 11 microservices:
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │────│ Product Catalog │────│ Recommendation │
│   (Web UI)      │    │    Service      │    │    Service      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Cart Service  │────│  Redis Cart     │    │   Ad Service    │
│                 │    │   (Database)    │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │
         ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ Checkout Service│────│ Payment Service │    │ Email Service   │
│                 │    │                 │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │
         ▼                       ▼
┌─────────────────┐    ┌─────────────────┐
│ Shipping Service│    │Currency Service │
│                 │    │                 │
└─────────────────┘    └─────────────────┘


🚀 Quick Start
Prerequisites
Ensure you have the following tools installed:

Kubernetes Cluster (minikube, kind, or cloud provider)
kubectl v1.20+
Helm v3.x
Helmfile (latest)
Docker (for building/testing)

Installation
bash# Clone the repository
git clone https://github.com/your-username/microservices-k8s-helm-tutorial.git
cd microservices-k8s-helm-tutorial

# Make scripts executable (Linux/macOS)
chmod +x install.sh uninstall.sh

# For Windows users, run PowerShell scripts
# .\install.ps1
Deploy the Application
bash# Option 1: Use automation script
./install.sh

# Option 2: Manual deployment with Helmfile
kubectl create namespace microservices-demo
helmfile sync

# Option 3: Individual Helm deployments
helm install frontend ./charts/microservice -f ./values/frontend-values.yaml -n microservices-demo
Access the Application
bash# Get the application URL
kubectl get services -n microservices-demo

# For minikube users
minikube service frontend -n microservices-demo

# For port forwarding
kubectl port-forward -n microservices-demo service/frontend 8080:80
Visit http://localhost:8080 to see the Hipster Shop application.


📁 Project Structure
microservices-k8s-helm-tutorial/
├── charts/                          # Helm Charts directory
│   ├── microservice/               # Generic microservice chart
│   │   ├── Chart.yaml             # Chart metadata
│   │   ├── values.yaml            # Default values
│   │   └── templates/             # Kubernetes templates
│   │       ├── deployment.yaml    # Deployment template
│   │       ├── service.yaml       # Service template
│   │       └── _helpers.tpl       # Template helpers
│   └── redis/                     # Redis-specific chart
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
├── values/                        # Values files for each service
│   ├── frontend-values.yaml
│   ├── cartservice-values.yaml
│   ├── checkout-service-values.yaml
│   ├── currency-service-values.yaml
│   ├── email-service-values.yaml
│   ├── payment-service-values.yaml
│   ├── productcatalog-service-values.yaml
│   ├── recommendation-service-values.yaml
│   ├── redis-values.yaml
│   └── shipping-service-values.yaml
├── config.yaml                   # Native Kubernetes manifests
├── helmfile.yaml                 # Helmfile configuration
├── install.sh                    # Installation script (Linux/macOS)
├── install.ps1                   # Installation script (Windows)
├── uninstall.sh                  # Cleanup script (Linux/macOS)
├── uninstall.ps1                 # Cleanup script (Windows)
└── README.md                     # This file

🔧 Prerequisites
Software Requirements
ToolVersionInstallationKubernetes1.20+Install Guidekubectl1.20+Install GuideHelm3.xInstall GuideHelmfileLatestInstall GuideDockerLatestInstall Guide
Platform-Specific Installation
Linux (Ubuntu/Debian)
bash# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Install Helm
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Install Helmfile
wget -O helmfile_linux_amd64 https://github.com/roboll/helmfile/releases/latest/download/helmfile_linux_amd64
chmod +x helmfile_linux_amd64
sudo mv helmfile_linux_amd64 /usr/local/bin/helmfile
macOS
bash# Using Homebrew
brew install kubectl helm helmfile

# Using MacPorts
sudo port install kubectl helm3
Windows
powershell# Using Chocolatey
choco install kubernetes-cli kubernetes-helm

# Using Scoop
scoop install kubectl helm helmfile

# Manual installation available from official websites

📚 Tutorial Phases
Phase 1: Native Kubernetes Deployment
Learn the fundamentals by deploying microservices using traditional YAML manifests.

Create namespace and deploy services manually
Understand service discovery and networking
Experience the complexity of managing multiple configurations
Troubleshoot common deployment issues

Key Files: config.yaml

Phase 2: Helm Charts Creation
Transform static YAML into reusable, templated Helm Charts.

Create generic microservice chart templates
Implement values-based configuration
Build specialized charts for stateful services (Redis)
Understand Helm templating and best practices

Key Files: charts/microservice/, charts/redis/

Phase 3: Helmfile Integration
Scale deployment management with Helmfile for multiple releases.

Configure declarative release management
Implement environment-specific values
Enable easy scaling and updates
Practice GitOps workflows

Key Files: helmfile.yaml, values/*.yaml