# ☁️ Cloud-Native Blog Platform on AWS EKS

<p align="center">
  <img src="https://miro.medium.com/v2/resize:fit:720/format:webp/0*LdTGlxSTQHnTl9uy.gif" alt="DevOps Pipeline" width="600"/>
</p>

<p align="center">
  <em>A Full-Stack Blogging App deployed on AWS EKS with end-to-end CI/CD, security scanning, and monitoring.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Java-17-ED8B00?style=flat-square&logo=openjdk&logoColor=white"/>
  <img src="https://img.shields.io/badge/Spring_Boot-3.3.2-6DB33F?style=flat-square&logo=spring-boot"/>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white"/>
  <img src="https://img.shields.io/badge/Terraform-7B42BC?style=flat-square&logo=terraform&logoColor=white"/>
  <img src="https://img.shields.io/badge/Jenkins-D24939?style=flat-square&logo=jenkins&logoColor=white"/>
  <img src="https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazonaws&logoColor=white"/>
</p>

---

## 📌 What This Project Does

This project takes a **Spring Boot blogging application** and wraps it in a **complete DevOps pipeline** — from code commit to production deployment on Kubernetes, with automated testing, security scanning, and monitoring at every step.

**In short:** Push code → Jenkins builds, tests, scans, containerizes, and deploys it to AWS EKS automatically.

---

## ✨ Key Features

- 📝 Create, edit & delete blog posts
- 🔐 User authentication with Spring Security
- 🐳 Dockerized & deployed to **AWS EKS**
- 🏗️ Infrastructure as Code with **Terraform**
- 🔄 Automated CI/CD via **Jenkins**
- 🔍 Code quality checks with **SonarQube**
- 🛡️ Vulnerability scanning with **Trivy**
- 📦 Artifact management with **Nexus**
- 📊 Monitoring with **Prometheus & Grafana**
- 📧 Email notifications on build status

---

## 📸 App Screenshots

| Login Page | Home Page | Create Post |
|:---:|:---:|:---:|
| ![Login](https://miro.medium.com/v2/resize:fit:700/0*1TkD_l1YWmE6CPwz.png) | ![Home](https://miro.medium.com/v2/resize:fit:700/0*pFYg1X3ZAZ0STgcS.png) | ![Post](https://miro.medium.com/v2/resize:fit:700/0*6156xWhmbdVtlxYj.png) |

---

## 🏗️ Architecture

```
  Developer → GitHub → Jenkins → Docker Hub → AWS EKS (Live App)
                          │
                ┌─────────┼─────────┐
                ▼         ▼         ▼
           SonarQube    Nexus     Trivy
           (Quality)  (Artifacts) (Security)

          Prometheus + Grafana (Monitoring)
```

---

## 🛠️ Tech Stack

| Category | Tool |
|----------|------|
| **Backend** | Java 17, Spring Boot 3.3.2 |
| **Frontend** | Thymeleaf |
| **Database** | H2 (In-Memory) |
| **Build** | Maven |
| **Container** | Docker |
| **Orchestration** | AWS EKS (Kubernetes) |
| **IaC** | Terraform |
| **CI/CD** | Jenkins |
| **Code Quality** | SonarQube |
| **Security** | Trivy |
| **Artifacts** | Nexus Repository |
| **Monitoring** | Prometheus, Grafana |

---

## 📁 Project Structure

```
cloud-native-blog-platform-eks/
├── README.md
└── FullStack-Blogging-App/
    ├── Dockerfile                 # Docker image definition
    ├── Jenkinsfile                # CI/CD pipeline
    ├── deployment-service.yml     # K8s Deployment + Service
    ├── pom.xml                    # Maven config
    ├── EKS_Terraform/
    │   ├── main.tf                # EKS cluster, VPC, IAM
    │   ├── variables.tf           # Input variables
    │   └── output.tf              # Output values
    └── src/                       # Spring Boot source code
```

---

## 🔄 CI/CD Pipeline

The Jenkins pipeline automates the entire delivery process:

```
Git Checkout → Compile → Test → Trivy FS Scan → SonarQube Analysis →
Quality Gate → Build JAR → Deploy to Nexus → Docker Build & Tag →
Docker Image Scan → Push to DockerHub → Deploy to K8s → Verify → Email
```

![Pipeline Stages](https://miro.medium.com/v2/resize:fit:700/0*BLOQKvQWc_cacpYd.png)

> 📄 Full pipeline code: [`Jenkinsfile`](./FullStack-Blogging-App/Jenkinsfile)

---

## 🚀 Getting Started

### Prerequisites

- AWS Account (with EKS, EC2, VPC, IAM permissions)
- Docker Hub account
- GitHub account
- 7 EC2 instances on AWS

### Quick Start

```bash
# Clone the repo
git clone https://github.com/Hemanshubt/cloud-native-blog-platform-eks.git
cd cloud-native-blog-platform-eks/FullStack-Blogging-App

# Provision EKS cluster
cd EKS_Terraform
terraform init && terraform apply --auto-approve

# Connect to EKS
aws eks --region ap-southeast-1 update-kubeconfig --name abrahimcse-cluster

# Create namespace & deploy
kubectl create ns webapps
# Apply RBAC manifests, then run the Jenkins pipeline
```

---

## ☁️ Infrastructure Setup

### EC2 Instances

You need **7 servers** on AWS:

| Server | Type | Storage | Role |
|--------|------|---------|------|
| Master Node | t2.medium | 25 GB | K8s Master |
| Slave-1 | t2.medium | 25 GB | K8s Worker |
| Slave-2 | t2.medium | 25 GB | K8s Worker |
| SonarQube | t2.medium | 25 GB | Code Analysis |
| Nexus | t2.medium | 25 GB | Artifact Repo |
| Monitoring | t2.medium | 25 GB | Prometheus + Grafana |
| Jenkins | **t2.large** | **30 GB** | CI/CD Server |

> **Ports to open:** 8080, 9000, 8081, 9090, 3000, 9100, 9115

![EC2 Instances](https://miro.medium.com/v2/resize:fit:700/0*HiK2H1lqLBR0Fs8r.png)

---

### EKS Cluster (Terraform)

<details>
<summary><b>1️⃣ Install AWS CLI</b></summary>

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

</details>

<details>
<summary><b>2️⃣ Configure AWS CLI</b></summary>

```bash
aws configure
# Enter: Access Key, Secret Key, Region (ap-southeast-1), Output (json)
```

</details>

<details>
<summary><b>3️⃣ Install Terraform</b></summary>

```bash
sudo apt install -y gnupg software-properties-common curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | \
  sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
  https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
  sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform -y
```

</details>

<details>
<summary><b>4️⃣ Create EKS Cluster</b></summary>

Terraform files are in [`EKS_Terraform/`](./FullStack-Blogging-App/EKS_Terraform/):

| File | What it creates |
|------|----------------|
| `main.tf` | VPC, Subnets, EKS Cluster, Node Group, IAM Roles |
| `variables.tf` | SSH key variable |
| `output.tf` | Cluster & network outputs |

```bash
cd FullStack-Blogging-App/EKS_Terraform
terraform init
terraform validate
terraform plan
terraform apply --auto-approve
```

![Terraform](https://miro.medium.com/v2/resize:fit:700/0*UBPDcX4x-2g4TTZt.png)

</details>

<details>
<summary><b>5️⃣ Connect to EKS</b></summary>

```bash
aws eks --region ap-southeast-1 update-kubeconfig --name abrahimcse-cluster
sudo snap install kubectl --classic
kubectl get nodes
```

![EKS Nodes](https://miro.medium.com/v2/resize:fit:700/0*iGVhn-oueNHapNHa.png)

</details>

---

### RBAC Setup (for Jenkins → K8s access)

<details>
<summary><b>📄 Create Service Account, Role, RoleBinding & Secret</b></summary>

```bash
kubectl create ns webapps
```

**svc.yaml** — Service Account:
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps
```

**role.yaml** — Role with full access to webapps namespace:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-role
  namespace: webapps
rules:
  - apiGroups: ["", "apps", "autoscaling", "batch", "extensions", "policy", "rbac.authorization.k8s.io"]
    resources: ["pods", "secrets", "configmaps", "deployments", "services", "replicasets", "namespaces", "events", "jobs", "serviceaccounts", "persistentvolumes", "persistentvolumeclaims"]
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```

**bind.yaml** — Bind role to service account:
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
  - kind: ServiceAccount
    name: jenkins
    namespace: webapps
```

**sec.yaml** — Token secret:
```yaml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: mysecretname
  annotations:
    kubernetes.io/service-account.name: jenkins
```

**Apply everything:**
```bash
kubectl apply -f svc.yaml
kubectl apply -f role.yaml
kubectl apply -f bind.yaml
kubectl apply -f sec.yaml -n webapps
```

**Get the token (add to Jenkins credentials):**
```bash
kubectl describe secret mysecretname -n webapps
```

</details>

<details>
<summary><b>🐳 Docker Registry Secret</b></summary>

```bash
kubectl create secret docker-registry regcred \
  --docker-server=https://index.docker.io/v1/ \
  --docker-username=<DOCKERHUB_USERNAME> \
  --docker-password=<DOCKERHUB_PASSWORD> \
  --docker-email=<YOUR_EMAIL> \
  --namespace=webapps
```

</details>

---

## 🔧 DevOps Tools Setup

### SonarQube

```bash
docker run -d --name Sonar -p 9000:9000 sonarqube:lts-community
```

- **URL:** `http://<IP>:9000` — Login: `admin` / `admin`
- Generate token: `Administration > Security > Users > Tokens`
- Add webhook: `Administration > Webhooks` → `http://<jenkins_ip>:8080/sonarqube-webhook/`

| Quality Gate | Issues |
|:---:|:---:|
| ![](https://miro.medium.com/v2/resize:fit:700/0*o4bizIr1lWAfvBYP.png) | ![](https://miro.medium.com/v2/resize:fit:700/0*FcKyyZkLm6SI_cCw.png) |

---

### Nexus Repository

```bash
docker run -d --name Nexus -p 8081:8081 sonatype/nexus3
```

- **URL:** `http://<IP>:8081` — Login: `admin` / *(get password below)*

```bash
docker exec -it <container_id> cat sonatype-work/nexus3/admin.password
```

Update `pom.xml` with your Nexus IP:

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

---

### Jenkins

<details>
<summary><b>📦 Install Jenkins + Trivy + kubectl</b></summary>

**Jenkins:**
```bash
sudo apt install openjdk-17-jre-headless -y
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | \
  sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update && sudo apt-get install jenkins -y
```

**Trivy:**
```bash
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | \
  gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] \
  https://aquasecurity.github.io/trivy-repo/deb generic main" | \
  sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update && sudo apt-get install trivy -y
```

**kubectl:**
```bash
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin
```

**Add Jenkins to Docker group:**
```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

</details>

**Access Jenkins:** `http://<IP>:8080` — Get password: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

#### Plugins to Install

> `Manage Jenkins > Plugins > Available`

Docker · Docker Pipeline · Kubernetes · Kubernetes CLI · Kubernetes Credentials · Prometheus Metrics · Pipeline Stage View · Pipeline Maven · Maven Integration · SonarQube Scanner · Config File Provider · Eclipse Temurin Installer

#### Tools Configuration

> `Manage Jenkins > Tools`

| Tool | Name | Version |
|------|------|---------|
| JDK | `jdk17` | Adoptium `jdk-17.0.9+9` |
| Maven | `maven3` | `3.6.1` |
| SonarQube Scanner | `sonar-scanner` | Latest |
| Docker | `docker` | Auto-install |

#### Credentials

> `Manage Jenkins > Credentials > Global`

| ID | Type | For |
|----|------|-----|
| `git-cred` | Username/Password | GitHub |
| `sonar-token` | Secret Text | SonarQube |
| `docker-cred` | Username/Password | Docker Hub |
| `k8-cred` | Secret Text | K8s Token |
| `mail-cred` | Username/Password | Gmail |

<details>
<summary><b>📄 Maven Settings (for Nexus)</b></summary>

> `Manage Jenkins > Managed Files > Add New Config` → Global Maven settings.xml (ID: `global-settings`)

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

</details>

<details>
<summary><b>📄 SonarQube Server Config</b></summary>

> `Manage Jenkins > System > SonarQube Servers`

| Field | Value |
|-------|-------|
| Name | `sonar` |
| URL | `http://<SONAR_IP>:9000` |
| Token | `sonar-token` |

</details>

---

### 📧 Email Notifications

1. Generate a [Gmail App Password](https://myaccount.google.com/apppasswords)
2. In Jenkins (`Manage Jenkins > System`):

| Setting | Value |
|---------|-------|
| SMTP Server | `smtp.gmail.com` |
| SMTP Port | `465` |
| Use SSL | ✅ |
| Credentials | `mail-cred` |

![Email Test](https://miro.medium.com/v2/resize:fit:700/0*UPTdaw9n3kgTRfsT.png)

---

## 📊 Monitoring Stack

### Prometheus

```bash
wget https://github.com/prometheus/prometheus/releases/download/v3.5.0-rc.0/prometheus-3.5.0-rc.0.linux-amd64.tar.gz
tar -xvf prometheus-*.tar.gz && rm prometheus-*.tar.gz
mv prometheus-* prometheus && cd prometheus
./prometheus &
```

**Access:** `http://<IP>:9090`

---

### Grafana

```bash
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/enterprise/release/grafana-enterprise_12.0.2_amd64.deb
sudo dpkg -i grafana-enterprise_12.0.2_amd64.deb
sudo systemctl start grafana-server
```

**Access:** `http://<IP>:3000` — Login: `admin` / `admin`

**Connect to Prometheus:** `Connections > Data Sources > Prometheus` → URL: `http://<PROMETHEUS_IP>:9090`

---

### Exporters

<details>
<summary><b>Blackbox Exporter</b> (on Monitoring server)</summary>

```bash
wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.27.0/blackbox_exporter-0.27.0.linux-amd64.tar.gz
tar -xvf blackbox_exporter-*.tar.gz && rm blackbox_exporter-*.tar.gz
mv blackbox_exporter-* blackbox_exporter && cd blackbox_exporter
./blackbox_exporter &
```

Add to `prometheus.yml`:
```yaml
- job_name: 'blackbox'
  metrics_path: /probe
  params:
    module: [http_2xx]
  static_configs:
    - targets:
      - http://prometheus.io
  relabel_configs:
    - source_labels: [__address__]
      target_label: __param_target
    - source_labels: [__param_target]
      target_label: instance
    - target_label: __address__
      replacement: 127.0.0.1:9115
```

</details>

<details>
<summary><b>Node Exporter</b> (on Jenkins server)</summary>

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
tar -xvf node_exporter-*.tar.gz && rm node_exporter-*.tar.gz
mv node_exporter-* node_exporter && cd node_exporter
./node_exporter &
```

Add to `prometheus.yml`:
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

### Import Grafana Dashboards

> `Dashboard > Import` → Enter Dashboard ID → Select Prometheus → Import

| Dashboard | ID |
|-----------|----|
| Blackbox Exporter | `7587` |
| Node Exporter | `1860` |

| Blackbox Dashboard | Node Exporter Dashboard |
|:---:|:---:|
| ![](https://miro.medium.com/v2/resize:fit:700/0*Cs-ibvi2BZCHh4x5.png) | ![](https://miro.medium.com/v2/resize:fit:700/0*f5KkcOraPpDhe7mg.png) |

---

## 📝 Conclusion

This project is a **complete DevOps CI/CD journey** — not just a blogging app. It covers infrastructure provisioning, CI/CD automation, security scanning, artifact management, container orchestration, and real-time monitoring using industry-standard tools.

Perfect for **portfolios**, **DevOps learning**, and **enterprise POCs**.

---

<p align="center">
  <b>⭐ Star this repo if you found it helpful!</b>
</p>
