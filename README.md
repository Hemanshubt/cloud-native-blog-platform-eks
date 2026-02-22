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

# ☁️ Infrastructure Setup (Step-by-Step)

## Step 1 — Launch EC2 Instances

Create **7 EC2 instances** on AWS:

| # | Server | Instance Type | Storage | Role |
|---|--------|------|---------|------|
| 1 | Master Node | t2.medium | 25 GB | K8s Master |
| 2 | Slave-1 | t2.medium | 25 GB | K8s Worker |
| 3 | Slave-2 | t2.medium | 25 GB | K8s Worker |
| 4 | SonarQube | t2.medium | 25 GB | Code Analysis |
| 5 | Nexus | t2.medium | 25 GB | Artifact Repo |
| 6 | Monitoring | t2.medium | 25 GB | Prometheus + Grafana |
| 7 | Jenkins | **t2.large** | **30 GB** | CI/CD Server |

> **Ports to open in Security Group:** `8080`, `9000`, `8081`, `9090`, `3000`, `9100`, `9115`

![EC2 Instances](https://miro.medium.com/v2/resize:fit:700/0*HiK2H1lqLBR0Fs8r.png)

---

## Step 2 — Install AWS CLI (on Master Node)

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

**Expected output:** `aws-cli/2.x.x Python/3.x.x Linux/...`

---

## Step 3 — Configure AWS CLI

```bash
aws configure
```

Enter the following when prompted:
- **AWS Access Key ID**
- **AWS Secret Access Key**
- **Region:** `ap-southeast-1`
- **Output format:** `json`

---

## Step 4 — Install Terraform

```bash
sudo apt install -y gnupg software-properties-common curl

curl -fsSL https://apt.releases.hashicorp.com/gpg | \
  sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
  https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
  sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update
sudo apt install terraform -y
terraform -version
```

---

## Step 5 — Create EKS Cluster with Terraform

Terraform config files are in [`EKS_Terraform/`](./FullStack-Blogging-App/EKS_Terraform/):

| File | What it creates |
|------|----------------|
| `main.tf` | VPC, Subnets, Security Groups, EKS Cluster, Node Group, IAM Roles |
| `variables.tf` | SSH key pair name |
| `output.tf` | Cluster ID, Node Group ID, VPC ID, Subnet IDs |

```bash
cd FullStack-Blogging-App/EKS_Terraform

terraform init
terraform validate
terraform plan
terraform apply --auto-approve
```

![Terraform Initialized](https://miro.medium.com/v2/resize:fit:700/0*UBPDcX4x-2g4TTZt.png)

![EKS Cluster Created](https://miro.medium.com/v2/resize:fit:700/0*YMydCBNOVdi_p_Rh.png)

---

## Step 6 — Connect to EKS Cluster

```bash
aws eks --region ap-southeast-1 update-kubeconfig --name abrahimcse-cluster
```

Install kubectl:

```bash
sudo snap install kubectl --classic
kubectl get nodes
```

![EKS Cluster Nodes](https://miro.medium.com/v2/resize:fit:700/0*iGVhn-oueNHapNHa.png)

![Node Group](https://miro.medium.com/v2/resize:fit:700/0*L-rZRSl9uz_Q2jg0.png)

---

## Step 7 — RBAC Setup (Jenkins → K8s Access)

Create a namespace for the application:

```bash
kubectl create ns webapps
```

### 7.1 — Create Service Account

Create `svc.yaml`:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps
```

```bash
kubectl apply -f svc.yaml
```

### 7.2 — Create Role

Create `role.yaml`:

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

```bash
kubectl apply -f role.yaml
```

### 7.3 — Bind Role to Service Account

Create `bind.yaml`:

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

```bash
kubectl apply -f bind.yaml
```

### 7.4 — Create Token Secret

Create `sec.yaml`:

```yaml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: mysecretname
  annotations:
    kubernetes.io/service-account.name: jenkins
```

```bash
kubectl apply -f sec.yaml -n webapps
```

### 7.5 — Get the Token

Copy this token and save it in Jenkins credentials later:

```bash
kubectl describe secret mysecretname -n webapps
```

### 7.6 — Create Docker Registry Secret

```bash
kubectl create secret docker-registry regcred \
  --docker-server=https://index.docker.io/v1/ \
  --docker-username=<DOCKERHUB_USERNAME> \
  --docker-password=<DOCKERHUB_PASSWORD> \
  --docker-email=<YOUR_EMAIL> \
  --namespace=webapps
```

Verify:

```bash
kubectl get secret regcred --namespace=webapps --output=yaml
```

---

# 🔧 DevOps Tools Setup (Step-by-Step)

## Step 8 — SonarQube Server

### 8.1 — Install Docker

```bash
sudo apt update
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### 8.2 — Run SonarQube

```bash
docker run -d --name Sonar -p 9000:9000 sonarqube:lts-community
```

### 8.3 — Access SonarQube

- **URL:** `http://<SONARQUBE_IP>:9000`
- **Username:** `admin`
- **Password:** `admin` *(change on first login)*

### 8.4 — Generate Token

1. Go to `Administration > Security > Users > Tokens`
2. Create a token named `sonar-token`
3. **Copy the token** — you'll need it in Jenkins

### 8.5 — Configure Webhook for Jenkins

1. Go to `Administration > Configuration > Webhooks`
2. Click **Create Webhook**
   - **Name:** `jenkins`
   - **URL:** `http://<JENKINS_IP>:8080/sonarqube-webhook/`

| Quality Gate Overview | Issues |
|:---:|:---:|
| ![](https://miro.medium.com/v2/resize:fit:700/0*o4bizIr1lWAfvBYP.png) | ![](https://miro.medium.com/v2/resize:fit:700/0*FcKyyZkLm6SI_cCw.png) |

---

## Step 9 — Nexus Repository Server

### 9.1 — Install Docker & Run Nexus

```bash
sudo apt update
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

docker run -d --name Nexus -p 8081:8081 sonatype/nexus3
```

### 9.2 — Get Admin Password

```bash
docker exec -it <container_id> cat sonatype-work/nexus3/admin.password
```

### 9.3 — Access Nexus

- **URL:** `http://<NEXUS_IP>:8081`
- **Username:** `admin`
- **Password:** *(from the command above)*

> ✅ Enable **anonymous access** if needed for open read access.

### 9.4 — Update pom.xml

Add your Nexus endpoints to the project's `pom.xml`:

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

## Step 10 — Jenkins Server

### 10.1 — Install Docker

```bash
sudo apt update
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### 10.2 — Install Trivy

```bash
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

### 10.3 — Install Jenkins

```bash
sudo apt install openjdk-17-jre-headless -y

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | \
  sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins -y
```

### 10.4 — Access Jenkins

- **URL:** `http://<JENKINS_IP>:8080`
- Get initial password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

![Unlock Jenkins](https://miro.medium.com/v2/resize:fit:700/0*yAkOzvp3d8of8guw.png)

### 10.5 — Install kubectl on Jenkins Server

```bash
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```

### 10.6 — Add Jenkins to Docker Group

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

---

# ⚙️ Jenkins Configuration (Step-by-Step)

## Step 11 — Install Plugins

Go to `Manage Jenkins > Plugins > Available Plugins` and install:

- ✅ Docker
- ✅ Docker Pipeline
- ✅ Kubernetes
- ✅ Kubernetes CLI
- ✅ Kubernetes Client API
- ✅ Kubernetes Credentials
- ✅ Prometheus Metrics
- ✅ Pipeline: Stage View
- ✅ Pipeline Maven Integration
- ✅ Maven Integration
- ✅ SonarQube Scanner
- ✅ Config File Provider
- ✅ Eclipse Temurin Installer

> ⚠️ **Restart Jenkins after installing all plugins.**

![Plugins](https://miro.medium.com/v2/resize:fit:700/0*N6yRKClIjMfgFEd8.png)

---

## Step 12 — Global Tool Configuration

Go to `Manage Jenkins > Tools`:

**JDK:**
- Name: `jdk17`
- Install automatically → Source: `Adoptium.net` → Version: `jdk-17.0.9+9`

**SonarQube Scanner:**
- Name: `sonar-scanner`
- Version: `Latest`

**Maven:**
- Name: `maven3`
- Version: `3.6.1`

**Docker:**
- Name: `docker`
- Install Automatically ✅

---

## Step 13 — Add Credentials

Go to `Manage Jenkins > Credentials > System > Global > Add Credentials`:

**GitHub:**
- Username: `<github-username>`
- Password: `<github-token>`
- ID: `git-cred`

**SonarQube:**
- Kind: `Secret text`
- Secret: `<sonar-token>`
- ID: `sonar-token`

**Docker Hub:**
- Username: `<dockerhub-username>`
- Password: `<dockerhub-password>`
- ID: `docker-cred`

**Kubernetes:**
- Kind: `Secret text`
- Secret: `<k8s-token>` *(from Step 7.5)*
- ID: `k8-cred`

**Gmail:**
- Kind: `Username with password`
- Username: `<your-email@gmail.com>`
- Password: `<Gmail App Password>`
- ID: `mail-cred`

![Credentials](https://miro.medium.com/v2/resize:fit:700/0*ripNKI_viCGmN1KO.png)

---

## Step 14 — Maven Settings for Nexus

Go to `Manage Jenkins > Managed Files > Add a New Config`:

- **Type:** Global Maven `settings.xml`
- **ID:** `global-settings`

Paste:

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

---

## Step 15 — SonarQube Server in Jenkins

Go to `Manage Jenkins > System > SonarQube Servers`:

- **Name:** `sonar`
- **Server URL:** `http://<SONARQUBE_IP>:9000`
- **Token:** `sonar-token` *(from credentials)*

---

## Step 16 — Create Pipeline Job

1. Go to Jenkins Dashboard → **New Item**
2. Name: `BloggingApp` → Type: **Pipeline** → Click **OK**
3. Discard Old Builds → Max # of builds: `2`
4. Pipeline Definition → Choose: **Pipeline script**
5. Paste the pipeline from [`Jenkinsfile`](./FullStack-Blogging-App/Jenkinsfile)

![Pipeline Stages](https://miro.medium.com/v2/resize:fit:700/0*BLOQKvQWc_cacpYd.png)

---

## Step 17 — Email Notification Setup (Gmail SMTP)

### 17.1 — Generate Gmail App Password

1. Go to [https://myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)
2. Enable **2-Step Verification** if not already
3. Create App Password for `Mail`
4. **Copy the password**

### 17.2 — Configure in Jenkins

Go to `Manage Jenkins > System`:

**Extended E-mail Notification:**
- SMTP Server: `smtp.gmail.com`
- SMTP Port: `465`
- ✅ Use SSL
- Credentials: `mail-cred`

**E-mail Notification:**
- SMTP Server: `smtp.gmail.com`
- SMTP Port: `465`
- ✅ Use SSL
- ✅ Use SMTP Authentication
- Username: `<your-email@gmail.com>`
- Password: `<Gmail App Password>`

### 17.3 — Test Configuration

Enter your email and click **Test Configuration** — you should receive a test email.

![Email Test](https://miro.medium.com/v2/resize:fit:700/0*UPTdaw9n3kgTRfsT.png)

---

# 📊 Monitoring Setup (Step-by-Step)

> All monitoring tools are installed on the **Monitoring server** unless noted otherwise.

## Step 18 — Install Prometheus

```bash
sudo apt update -y

wget https://github.com/prometheus/prometheus/releases/download/v3.5.0-rc.0/prometheus-3.5.0-rc.0.linux-amd64.tar.gz
tar -xvf prometheus-3.5.0-rc.0.linux-amd64.tar.gz
rm -rf prometheus-3.5.0-rc.0.linux-amd64.tar.gz
mv prometheus-3.5.0-rc.0.linux-amd64 prometheus
cd prometheus
./prometheus &
```

**Access:** `http://<MONITORING_IP>:9090`

---

## Step 19 — Install Grafana

```bash
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/enterprise/release/grafana-enterprise_12.0.2_amd64.deb
sudo dpkg -i grafana-enterprise_12.0.2_amd64.deb
sudo systemctl start grafana-server
```

- **URL:** `http://<MONITORING_IP>:3000`
- **Username:** `admin`
- **Password:** `admin`

![Grafana](https://miro.medium.com/v2/resize:fit:700/0*f7Fzx-vV823U0p_a.png)

---

## Step 20 — Install Blackbox Exporter

```bash
wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.27.0/blackbox_exporter-0.27.0.linux-amd64.tar.gz
tar -xvf blackbox_exporter-0.27.0.linux-amd64.tar.gz
rm -rf blackbox_exporter-0.27.0.linux-amd64.tar.gz
mv blackbox_exporter-0.27.0.linux-amd64 blackbox_exporter
cd blackbox_exporter
./blackbox_exporter &
```

**Access:** `http://<MONITORING_IP>:9115`

Add this job to `prometheus.yml`:

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

---

## Step 21 — Install Node Exporter (on Jenkins Server)

> ⚠️ This is installed on the **Jenkins server**, not the Monitoring server.

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
tar -xvf node_exporter-1.9.1.linux-amd64.tar.gz
rm -rf node_exporter-1.9.1.linux-amd64.tar.gz
mv node_exporter-1.9.1.linux-amd64 node_exporter
cd node_exporter
./node_exporter &
```

**Access:** `http://<JENKINS_IP>:9100`

Add these jobs to `prometheus.yml` (on Monitoring server):

```yaml
- job_name: 'node_exporter'
  static_configs:
    - targets: ['<JENKINS_IP>:9100']

- job_name: 'jenkins'
  metrics_path: /prometheus
  static_configs:
    - targets: ['<JENKINS_IP>:8080']
```

**Restart Prometheus after config changes:**

```bash
pgrep prometheus
kill <PID>
./prometheus &
```

---

## Step 22 — Connect Grafana to Prometheus

1. Go to `Grafana > Connections > Data Sources > Add data source`
2. Select **Prometheus**
3. URL: `http://<MONITORING_IP>:9090`
4. Click **Save & Test** → should show *"Data source is working"*

---

## Step 23 — Import Grafana Dashboards

1. Go to `Dashboard > Import`
2. Enter the Dashboard ID
3. Select **Prometheus** as data source
4. Click **Import**

| Dashboard | ID |
|-----------|----|
| 🔍 Blackbox Exporter | `7587` |
| 🖥️ Node Exporter | `1860` |

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
