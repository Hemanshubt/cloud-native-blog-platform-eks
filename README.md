# ☁️ Cloud-Native Blog Platform on AWS EKS

<p align="center">
  <img src="https://miro.medium.com/v2/resize:fit:720/format:webp/0*LdTGlxSTQHnTl9uy.gif" alt="DevOps Pipeline" width="600"/>
</p>

<p align="center">
  <strong>A Production-Grade Full-Stack Blogging Application deployed on AWS EKS with a complete CI/CD pipeline</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Spring_Boot-3.3.2-6DB33F?style=for-the-badge&logo=spring-boot" alt="Spring Boot"/>
  <img src="https://img.shields.io/badge/Java-17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white" alt="Java"/>
  <img src="https://img.shields.io/badge/Docker-Container-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker"/>
  <img src="https://img.shields.io/badge/Kubernetes-EKS-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white" alt="Kubernetes"/>
  <img src="https://img.shields.io/badge/Terraform-IaC-7B42BC?style=for-the-badge&logo=terraform&logoColor=white" alt="Terraform"/>
  <img src="https://img.shields.io/badge/Jenkins-CI/CD-D24939?style=for-the-badge&logo=jenkins&logoColor=white" alt="Jenkins"/>
</p>

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Architecture](#-architecture)
- [Application Screenshots](#-application-screenshots)
- [Infrastructure Setup](#-infrastructure-setup)
  - [AWS Setup](#1-aws-setup)
  - [EKS Cluster via Terraform](#2-eks-cluster-via-terraform)
  - [RBAC Configuration](#3-rbac-configuration)
- [DevOps Tools Setup](#-devops-tools-setup)
  - [SonarQube](#1-sonarqube-server)
  - [Nexus Repository](#2-nexus-repository)
  - [Jenkins](#3-jenkins-server)
- [CI/CD Pipeline](#-cicd-pipeline)
  - [Pipeline Stages](#pipeline-stages)
  - [Jenkins Configuration](#jenkins-configuration)
  - [Email Notifications](#email-notifications)
- [Monitoring Stack](#-monitoring-stack)
  - [Prometheus](#1-prometheus)
  - [Grafana](#2-grafana)
  - [Blackbox Exporter](#3-blackbox-exporter)
  - [Node Exporter](#4-node-exporter)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)

---

## 🎯 Project Overview

A production-ready **Full-Stack Blogging Application** built with **Java (Spring Boot)**, containerized with **Docker**, and deployed on **AWS EKS (Elastic Kubernetes Service)** — fully automated using modern DevOps tools and practices.

This project demonstrates an end-to-end DevOps workflow, including infrastructure provisioning with Terraform, CI/CD automation with Jenkins, static code analysis, vulnerability scanning, artifact management, and real-time monitoring.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 📝 **Blog Management** | Create, edit, and delete blog posts with a clean UI |
| 🔐 **Authentication** | User login and registration with Spring Security |
| 🌐 **RESTful API** | Backend API powered by Spring Boot |
| 🐳 **Containerized** | Multi-stage Docker builds for optimized images |
| ☸️ **Kubernetes** | Deployed on AWS EKS with auto-scaling |
| 🏗️ **IaC** | Infrastructure provisioned via Terraform |
| 🔄 **CI/CD** | Fully automated Jenkins pipeline |
| 🔍 **Code Quality** | Static analysis with SonarQube |
| 🛡️ **Security** | Vulnerability scanning with Trivy (FS + Image) |
| 📦 **Artifacts** | Maven artifact storage via Nexus Repository |
| 📊 **Monitoring** | Prometheus + Grafana dashboards |
| 📧 **Notifications** | Email alerts on deployment status via Gmail SMTP |

---

## 🛠️ Tech Stack

```
┌──────────────────────────────────────────────────────────────────┐
│                        TECH STACK                                │
├───────────────┬──────────────────────────────────────────────────┤
│ Language      │ Java 17                                          │
│ Framework     │ Spring Boot 3.3.2 (Web, JPA, Security, Thymeleaf)│
│ Database      │ H2 (In-Memory)                                   │
│ Build Tool    │ Maven 3.6+                                       │
│ Container     │ Docker (Eclipse Temurin 17 Alpine)                │
│ Orchestration │ Kubernetes (AWS EKS)                              │
│ IaC           │ Terraform                                        │
│ CI/CD         │ Jenkins (Declarative Pipeline)                    │
│ Code Quality  │ SonarQube (LTS Community)                        │
│ Security Scan │ Trivy                                            │
│ Artifacts     │ Nexus Repository Manager 3                       │
│ Monitoring    │ Prometheus, Grafana, Blackbox & Node Exporter     │
│ Cloud         │ AWS (EC2, EKS, VPC, IAM)                         │
└───────────────┴──────────────────────────────────────────────────┘
```

---

## 🏗️ Architecture

```
┌─────────────┐     ┌──────────┐     ┌──────────┐     ┌────────────┐     ┌──────────────┐
│  Developer  │────▶│  GitHub   │────▶│ Jenkins  │────▶│ Docker Hub │────▶│  AWS EKS     │
│  (Git Push) │     │  (SCM)   │     │ (CI/CD)  │     │ (Registry) │     │  (K8s Cluster)│
└─────────────┘     └──────────┘     └────┬─────┘     └────────────┘     └──────────────┘
                                          │
                              ┌───────────┼───────────┐
                              ▼           ▼           ▼
                        ┌──────────┐ ┌─────────┐ ┌─────────┐
                        │SonarQube │ │  Nexus  │ │  Trivy  │
                        │(Quality) │ │(Artifact)│ │ (Scan)  │
                        └──────────┘ └─────────┘ └─────────┘

                    ┌──────────────────────────────────────┐
                    │         Monitoring Stack              │
                    │  Prometheus ──▶ Grafana               │
                    │  Blackbox Exporter | Node Exporter    │
                    └──────────────────────────────────────┘
```

---

## 📸 Application Screenshots

<table>
  <tr>
    <td align="center"><strong>Login Page</strong></td>
    <td align="center"><strong>Home Page</strong></td>
    <td align="center"><strong>Create Post</strong></td>
  </tr>
  <tr>
    <td><img src="https://miro.medium.com/v2/resize:fit:700/0*1TkD_l1YWmE6CPwz.png" width="300"/></td>
    <td><img src="https://miro.medium.com/v2/resize:fit:700/0*pFYg1X3ZAZ0STgcS.png" width="300"/></td>
    <td><img src="https://miro.medium.com/v2/resize:fit:700/0*6156xWhmbdVtlxYj.png" width="300"/></td>
  </tr>
</table>

---

## 🏢 Infrastructure Setup

### 1. AWS Setup

**EC2 Instances Required:** 7 servers

| Server | Instance Type | Storage | Purpose |
|--------|--------------|---------|---------|
| Master Node | t2.medium | 25 GB | K8s Master |
| Slave-1 | t2.medium | 25 GB | K8s Worker Node |
| Slave-2 | t2.medium | 25 GB | K8s Worker Node |
| SonarQube | t2.medium | 25 GB | Code Quality Analysis |
| Nexus | t2.medium | 25 GB | Artifact Repository |
| Monitoring | t2.medium | 25 GB | Prometheus + Grafana |
| Jenkins | **t2.large** | **30 GB** | CI/CD Server |

> [!NOTE]
> Use the default VPC with a Security Group that has all required ports open (8080, 9000, 8081, 9090, 3000, 9100, 9115).

![EC2 Instances](https://miro.medium.com/v2/resize:fit:700/0*HiK2H1lqLBR0Fs8r.png)

---

### 2. EKS Cluster via Terraform

#### Step 1: Install AWS CLI

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
# Expected: aws-cli/2.x.x Python/3.x.x Linux/...
```

#### Step 2: Configure AWS CLI

```bash
aws configure
```

Provide: **Access Key ID**, **Secret Access Key**, **Region** (e.g., `ap-southeast-1`), **Output** (`json`).

#### Step 3: Install Terraform

**Method 1: Official APT Repository (Recommended)**

```bash
sudo apt install -y gnupg software-properties-common curl

curl -fsSL https://apt.releases.hashicorp.com/gpg | \
  sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
  https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
  sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update && sudo apt install terraform -y
terraform -version
```

**Method 2: Snap**

```bash
sudo snap install terraform --classic
```

#### Step 4: Provision EKS Cluster

The Terraform configuration files are located in [`FullStack-Blogging-App/EKS_Terraform/`](./FullStack-Blogging-App/EKS_Terraform/):

| File | Purpose |
|------|---------|
| `main.tf` | VPC, Subnets, Security Groups, EKS Cluster, Node Group, IAM Roles |
| `variables.tf` | SSH key pair name variable |
| `output.tf` | Cluster ID, Node Group ID, VPC ID, Subnet IDs |

```bash
cd FullStack-Blogging-App/EKS_Terraform

terraform init
terraform validate
terraform plan
terraform apply --auto-approve
```

![Terraform Init](https://miro.medium.com/v2/resize:fit:700/0*UBPDcX4x-2g4TTZt.png)

![EKS Cluster Created](https://miro.medium.com/v2/resize:fit:700/0*YMydCBNOVdi_p_Rh.png)

#### Step 5: Connect to EKS Cluster

```bash
aws eks --region ap-southeast-1 update-kubeconfig --name abrahimcse-cluster

sudo snap install kubectl --classic
kubectl get nodes
```

![EKS Cluster](https://miro.medium.com/v2/resize:fit:700/0*iGVhn-oueNHapNHa.png)

![Node Group](https://miro.medium.com/v2/resize:fit:700/0*L-rZRSl9uz_Q2jg0.png)

---

### 3. RBAC Configuration

Set up Role-Based Access Control for Jenkins to deploy to the EKS cluster.

```bash
# Create namespace
kubectl create ns webapps
```

<details>
<summary><strong>📄 Service Account (svc.yaml)</strong></summary>

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps
```

</details>

<details>
<summary><strong>📄 Role (role.yaml)</strong></summary>

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-role
  namespace: webapps
rules:
  - apiGroups:
      - ""
      - apps
      - autoscaling
      - batch
      - extensions
      - policy
      - rbac.authorization.k8s.io
    resources:
      - pods
      - secrets
      - componentstatuses
      - configmaps
      - daemonsets
      - deployments
      - events
      - endpoints
      - horizontalpodautoscalers
      - ingress
      - jobs
      - limitranges
      - namespaces
      - nodes
      - persistentvolumes
      - persistentvolumeclaims
      - resourcequotas
      - replicasets
      - replicationcontrollers
      - serviceaccounts
      - services
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```

</details>

<details>
<summary><strong>📄 RoleBinding (bind.yaml)</strong></summary>

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-rolebinding
  namespace: webapps
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: app-role
subjects:
  - namespace: webapps
    kind: ServiceAccount
    name: jenkins
```

</details>

<details>
<summary><strong>📄 Secret for Service Account Token (sec.yaml)</strong></summary>

```yaml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: mysecretname
  annotations:
    kubernetes.io/service-account.name: jenkins
```

</details>

**Apply all RBAC resources:**

```bash
kubectl apply -f svc.yaml
kubectl apply -f role.yaml
kubectl apply -f bind.yaml
kubectl apply -f sec.yaml -n webapps
```

**Retrieve the token (save it in Jenkins credentials):**

```bash
kubectl describe secret mysecretname -n webapps
```

**Create Docker Registry Secret:**

```bash
kubectl create secret docker-registry regcred \
  --docker-server=https://index.docker.io/v1/ \
  --docker-username=<DOCKERHUB_USERNAME> \
  --docker-password=<DOCKERHUB_PASSWORD> \
  --docker-email=<YOUR_EMAIL> \
  --namespace=webapps
```

---

## 🔧 DevOps Tools Setup

### 1. SonarQube Server

```bash
# Install Docker
sudo apt update
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Run SonarQube
docker run -d --name Sonar -p 9000:9000 sonarqube:lts-community
```

| | Details |
|---|---|
| **URL** | `http://<server_ip>:9000` |
| **Username** | `admin` |
| **Password** | `admin` (change on first login) |

**Generate Token:** `Administration > Security > Users > Tokens` → Create token named `sonar-token`

**Configure Webhook:** `Administration > Configuration > Webhooks` → URL: `http://<jenkins_ip>:8080/sonarqube-webhook/`

<table>
  <tr>
    <td><img src="https://miro.medium.com/v2/resize:fit:700/0*o4bizIr1lWAfvBYP.png" width="400"/><br/><em>Quality Gate Overview</em></td>
    <td><img src="https://miro.medium.com/v2/resize:fit:700/0*FcKyyZkLm6SI_cCw.png" width="400"/><br/><em>Issues</em></td>
  </tr>
</table>

---

### 2. Nexus Repository

```bash
# Run Nexus
docker run -d --name Nexus -p 8081:8081 sonatype/nexus3
```

**Retrieve admin password:**

```bash
docker exec -it <container_id> sh
cat sonatype-work/nexus3/admin.password
```

| | Details |
|---|---|
| **URL** | `http://<server_ip>:8081` |
| **Username** | `admin` |
| **Password** | *(from the file above)* |

> [!TIP]
> Enable **anonymous access** if open read access is needed for testing.

**Update `pom.xml`** with your Nexus repository endpoints:

```xml
<distributionManagement>
    <repository>
        <id>maven-releases</id>
        <url>http://<NEXUS_IP>:8081/repository/maven-releases/</url>
    </repository>
    <snapshotRepository>
        <id>maven-snapshots</id>
        <url>http://<NEXUS_IP>:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

![Nexus Browse](https://miro.medium.com/v2/resize:fit:700/0*PT4j1js8LENhXXiV.png)

---

### 3. Jenkins Server

#### Install Docker + Trivy + Jenkins

<details>
<summary><strong>📦 Install Trivy</strong></summary>

```bash
#!/bin/bash
sudo apt-get install wget gnupg
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | \
  gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] \
  https://aquasecurity.github.io/trivy-repo/deb generic main" | \
  sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
trivy --version
```

</details>

<details>
<summary><strong>📦 Install Jenkins</strong></summary>

```bash
#!/bin/bash
sudo apt install openjdk-17-jre-headless -y

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | \
  sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins -y
```

</details>

**Access Jenkins:** `http://<server_ip>:8080`

```bash
# Get initial admin password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

**Install kubectl on Jenkins server:**

```bash
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```

**Add Jenkins to Docker group:**

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

---

## 🔄 CI/CD Pipeline

### Pipeline Stages

```
Git Checkout → Compile → Test → Trivy FS Scan → SonarQube Analysis
     → Quality Gate → Build JAR → Deploy to Nexus → Docker Build
          → Docker Image Scan → Push to DockerHub
               → Deploy to Kubernetes → Verify Deployment
                    → Email Notification
```

![Pipeline Stages](https://miro.medium.com/v2/resize:fit:700/0*BLOQKvQWc_cacpYd.png)

> [!NOTE]
> The full pipeline code is in the [`Jenkinsfile`](./FullStack-Blogging-App/Jenkinsfile). Modify it according to your requirements.

---

### Jenkins Configuration

#### Required Plugins

Install from `Manage Jenkins > Plugins > Available`:

| Plugin | Purpose |
|--------|---------|
| Docker, Docker Pipeline | Docker build & push |
| Kubernetes, Kubernetes CLI, Client API, Credentials | K8s deployment |
| Prometheus Metrics | Monitoring integration |
| Pipeline: Stage View | Visual pipeline stages |
| Pipeline Maven Integration, Maven Integration | Maven builds |
| SonarQube Scanner | Code quality analysis |
| Config File Provider | Maven settings for Nexus |
| Eclipse Temurin Installer | JDK management |

> [!IMPORTANT]
> Restart Jenkins after installing all plugins.

#### Global Tool Configuration

Navigate to `Manage Jenkins > Tools`:

| Tool | Name | Version / Details |
|------|------|-------------------|
| **JDK** | `jdk17` | Adoptium.net — `jdk-17.0.9+9` |
| **SonarQube Scanner** | `sonar-scanner` | Latest |
| **Maven** | `maven3` | `3.6.1` |
| **Docker** | `docker` | Install Automatically |

#### Credentials Setup

Navigate to `Manage Jenkins > Credentials > System > Global`:

| ID | Kind | Description |
|----|------|-------------|
| `git-cred` | Username/Password | GitHub credentials |
| `sonar-token` | Secret Text | SonarQube authentication token |
| `docker-cred` | Username/Password | Docker Hub credentials |
| `k8-cred` | Secret Text | K8s service account token |
| `mail-cred` | Username/Password | Gmail App Password |

![Credentials](https://miro.medium.com/v2/resize:fit:700/0*ripNKI_viCGmN1KO.png)

#### Maven Settings (for Nexus)

Navigate to `Manage Jenkins > Managed Files > Add New Config`:

- **Type:** Global Maven `settings.xml`
- **ID:** `global-settings`

```xml
<settings>
  <servers>
    <server>
      <id>maven-releases</id>
      <username>NEXUS_USERNAME</username>
      <password>NEXUS_PASSWORD</password>
    </server>
    <server>
      <id>maven-snapshots</id>
      <username>NEXUS_USERNAME</username>
      <password>NEXUS_PASSWORD</password>
    </server>
  </servers>
</settings>
```

#### SonarQube Server Configuration

Navigate to `Manage Jenkins > System > SonarQube Servers`:

| Field | Value |
|-------|-------|
| **Name** | `sonar` |
| **Server URL** | `http://<SONAR_IP>:9000` |
| **Token** | `sonar-token` *(from credentials)* |

---

### Email Notifications

#### Step 1: Generate Gmail App Password

1. Go to [Google App Passwords](https://myaccount.google.com/apppasswords)
2. Enable **2-Step Verification** if not already
3. Generate an App Password for `Mail`

#### Step 2: Configure in Jenkins

Navigate to `Manage Jenkins > System`:

**Extended E-mail Notification:**

| Setting | Value |
|---------|-------|
| SMTP Server | `smtp.gmail.com` |
| SMTP Port | `465` |
| Use SSL | ✅ |
| Credentials | `mail-cred` |

**E-mail Notification:**

| Setting | Value |
|---------|-------|
| SMTP Server | `smtp.gmail.com` |
| SMTP Port | `465` |
| Use SSL | ✅ |
| Use SMTP Auth | ✅ |

![Email Test](https://miro.medium.com/v2/resize:fit:700/0*UPTdaw9n3kgTRfsT.png)

---

## 📊 Monitoring Stack

### 1. Prometheus

```bash
wget https://github.com/prometheus/prometheus/releases/download/v3.5.0-rc.0/prometheus-3.5.0-rc.0.linux-amd64.tar.gz
tar -xvf prometheus-3.5.0-rc.0.linux-amd64.tar.gz
rm -rf prometheus-3.5.0-rc.0.linux-amd64.tar.gz
mv prometheus-3.5.0-rc.0.linux-amd64 prometheus
cd prometheus
./prometheus &
```

**Access:** `http://<public_ip>:9090`

---

### 2. Grafana

```bash
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/enterprise/release/grafana-enterprise_12.0.2_amd64.deb
sudo dpkg -i grafana-enterprise_12.0.2_amd64.deb
sudo systemctl start grafana-server
```

| | Details |
|---|---|
| **URL** | `http://<public_ip>:3000` |
| **Username** | `admin` |
| **Password** | `admin` |

![Grafana Dashboard](https://miro.medium.com/v2/resize:fit:700/0*f7Fzx-vV823U0p_a.png)

---

### 3. Blackbox Exporter

```bash
wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.27.0/blackbox_exporter-0.27.0.linux-amd64.tar.gz
tar -xvf blackbox_exporter-0.27.0.linux-amd64.tar.gz
rm -rf blackbox_exporter-0.27.0.linux-amd64.tar.gz
mv blackbox_exporter-0.27.0.linux-amd64 blackbox_exporter
cd blackbox_exporter
./blackbox_exporter &
```

**Access:** `http://<public_ip>:9115`

<details>
<summary><strong>📄 Add to prometheus.yml</strong></summary>

```yaml
- job_name: 'blackbox'
  metrics_path: /probe
  params:
    module: [http_2xx]
  static_configs:
    - targets:
      - http://prometheus.io
      - http://example.com:8080
  relabel_configs:
    - source_labels: [__address__]
      target_label: __param_target
    - source_labels: [__param_target]
      target_label: instance
    - target_label: __address__
      replacement: 127.0.0.1:9115
```

</details>

---

### 4. Node Exporter

> [!NOTE]
> Install Node Exporter **on the Jenkins server** for system metrics.

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
tar -xvf node_exporter-1.9.1.linux-amd64.tar.gz
rm -rf node_exporter-1.9.1.linux-amd64.tar.gz
mv node_exporter-1.9.1.linux-amd64 node_exporter
cd node_exporter
./node_exporter &
```

**Access:** `http://<jenkins_ip>:9100`

<details>
<summary><strong>📄 Add to prometheus.yml</strong></summary>

```yaml
- job_name: 'node_exporter'
  static_configs:
    - targets: ['<JENKINS_IP>:9100']

- job_name: 'jenkins'
  metrics_path: /prometheus
  static_configs:
    - targets: ['<JENKINS_IP>:8080']
```

</details>

**Restart Prometheus after config changes:**

```bash
pgrep prometheus
kill <PID>
./prometheus &
```

### 5. Connect Grafana to Prometheus

1. Go to `Grafana > Connections > Data Sources > Add data source`
2. Select **Prometheus**
3. Set URL: `http://<PROMETHEUS_IP>:9090`
4. Click **Save & Test** → should see *"Data source is working"*

### 6. Import Dashboards

Navigate to `Dashboard > Import` and use these dashboard IDs:

| Dashboard | ID |
|-----------|----|
| 🔍 Blackbox Exporter | `7587` |
| 🖥️ Node Exporter | `1860` |

<table>
  <tr>
    <td><img src="https://miro.medium.com/v2/resize:fit:700/0*Cs-ibvi2BZCHh4x5.png" width="400"/><br/><em>Blackbox Exporter Dashboard</em></td>
    <td><img src="https://miro.medium.com/v2/resize:fit:700/0*f5KkcOraPpDhe7mg.png" width="400"/><br/><em>Node Exporter Dashboard</em></td>
  </tr>
</table>

---

## 📁 Project Structure

```
cloud-native-blog-platform-eks/
├── README.md
└── FullStack-Blogging-App/
    ├── Dockerfile                  # Multi-stage Docker build
    ├── Jenkinsfile                 # Declarative CI/CD pipeline
    ├── deployment-service.yml      # K8s Deployment + LoadBalancer Service
    ├── pom.xml                     # Maven project config with Nexus endpoints
    ├── mvnw / mvnw.cmd             # Maven wrapper scripts
    ├── EKS_Terraform/
    │   ├── main.tf                 # VPC, Subnets, EKS, IAM, Node Groups
    │   ├── variables.tf            # SSH key variable
    │   └── output.tf               # Cluster & network output values
    └── src/                        # Spring Boot application source code
```

---

## 🚀 Getting Started

### Prerequisites

- AWS Account with IAM permissions for EKS, EC2, VPC
- Docker Hub account
- GitHub repository with the blogging app code
- 7 EC2 instances (see [Infrastructure Setup](#1-aws-setup))

### Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/cloud-native-blog-platform-eks.git
cd cloud-native-blog-platform-eks/FullStack-Blogging-App

# 2. Provision EKS Cluster
cd EKS_Terraform
terraform init && terraform apply --auto-approve

# 3. Configure kubeconfig
aws eks --region ap-southeast-1 update-kubeconfig --name abrahimcse-cluster

# 4. Set up RBAC and deploy
kubectl create ns webapps
# Apply RBAC manifests (see RBAC Configuration section)

# 5. Build and deploy via Jenkins Pipeline
# Configure Jenkins with all plugins, credentials, and tools
# Run the pipeline — it handles build, test, scan, and deploy automatically
```

---

## 📝 Conclusion

This project is not just a simple CRUD **Full-Stack Blogging Application** — it's a **complete DevOps CI/CD journey** packaged with real-world production tools. It covers infrastructure provisioning, continuous integration, security scanning, artifact management, container orchestration, and monitoring — making it ideal for **portfolios**, **enterprise POCs**, and **DevOps learners** who want to understand how real-world systems operate.

---

<p align="center">
  <strong>⭐ Star this repo if you found it useful!</strong>
</p>
