<div align="center">

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                        HERO BANNER                        -->
<!-- ═══════════════════════════════════════════════════════════ -->

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:161b22,100:1f6feb&height=220&section=header&text=Gig%20Router&fontSize=72&fontColor=58a6ff&fontAlignY=35&desc=Production-Grade%20DevOps%20Engineering%20%E2%80%A2%20End-to-End%20Cloud%20Native%20Platform&descSize=16&descAlignY=55&descAlign=50&animation=fadeIn" width="100%"/>

<br/>

![AWS](https://img.shields.io/badge/AWS-EKS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-IaC-844FBA?style=for-the-badge&logo=terraform&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestration-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![ArgoCD](https://img.shields.io/badge/Argo%20CD-GitOps-EF7B4D?style=for-the-badge&logo=argo&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containers-2496ED?style=for-the-badge&logo=docker&logoColor=white)

<br/>


<p align="center">
  <a href="https://drive.google.com/file/d/1RcJ6tCCpsDGOkdXlso9ewRFzWt6CdEnW/view?usp=drive_link">
    <img src="https://img.shields.io/badge/Project%20Documentation-00796B?style=flat-square&logo=googledrive&logoColor=white" style="height:34px; object-fit:contain;"/>
  </a>
</p>

> **A comprehensive DevOps practice project** demonstrating the full lifecycle of building, securing, deploying, and operating a cloud-native application on **AWS EKS** — from infrastructure provisioning with Terraform, through multi-stage CI/CD pipelines, to GitOps-driven Kubernetes deployments with observability, logging, and intelligent automation.

<br/>

</div>

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                    TABLE OF CONTENTS                       -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 📑 Table of Contents

<table>
<tr>
<td width="50%">

- [🏗️ Architecture Overview](#%EF%B8%8F-architecture-overview)
- [📦 Repository Map](#-repository-map)
- [☁️ Infrastructure Layer — Terraform](#%EF%B8%8F-infrastructure-layer--terraform)
- [🔄 CI/CD Pipelines — Jenkins](#-cicd-pipelines--jenkins)
- [🚀 GitOps & Deployment — Argo CD](#-gitops--deployment--argo-cd)

</td>
<td width="50%">

- [📊 Observability Stack](#-observability-stack)
- [📝 Centralized Logging — EFK](#-centralized-logging--efk)
- [🤖 Intelligent Automation — n8n](#-intelligent-automation--n8n)
- [🔐 Security Practices](#-security-practices)
- [👥 Team & Purpose](#-team--purpose)

</td>
</tr>
</table>

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                  ARCHITECTURE OVERVIEW                     -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 🏗️ Architecture Overview

<div align="center">

```mermaid
%%{init: {'theme': 'dark', 'themeVariables': {'fontFamily': 'monospace'}}}%%
flowchart TB
    subgraph DEV["🧑‍💻 Developer Workflow"]
        direction LR
        CODE[Push Code] --> GH[GitHub]
    end

    subgraph INFRA["☁️ AWS Infrastructure · Terraform"]
        direction TB
        VPC["🌐 VPC · Subnets · SGs"]
        EKS["⎈ EKS Cluster"]
        RDS["🗄️ RDS PostgreSQL"]
        ECR["📦 ECR Registry"]
        SM["🔑 Secrets Manager"]
        IAM["👤 IAM · IRSA"]
        VPC --> EKS
        VPC --> RDS
        IAM --> EKS
    end

    subgraph CI["🔄 Jenkins CI/CD"]
        direction TB
        TEST["✅ Unit Tests\n+ Coverage"]
        SONAR["🔍 SonarQube\nQuality Gate"]
        OWASP["🛡️ OWASP\nDep-Check"]
        KANIKO["🐳 Kaniko\nBuild"]
        TRIVY["🔒 Trivy\nImage Scan"]
        NEXUS["📚 Nexus\nArtifacts"]
        TEST --> SONAR --> OWASP --> KANIKO --> TRIVY
        KANIKO --> NEXUS
    end

    subgraph CD["🚀 GitOps · Argo CD"]
        direction TB
        ARGO["Argo CD\nApp-of-Apps"]
        SYNC["Auto Sync\n+ Self-Heal"]
        ARGO --> SYNC
    end

    subgraph K8S["⎈ Kubernetes Workloads"]
        direction TB
        FE["⚛️ Frontend\nReact · Nginx"]
        BE["🐍 Backend\nDjango · Gunicorn"]
        REDIS["⚡ Redis\nCache"]
        MC["💾 Memcached"]
        FE -->|"/api proxy"| BE
        BE --> REDIS
    end

    subgraph OBS["📊 Observability"]
        direction LR
        PROM["Prometheus"]
        GRAF["Grafana"]
        EFK_S["EFK Stack"]
        PROM --> GRAF
    end

    subgraph NOTIFY["🤖 Automation"]
        N8N["n8n Workflows\nSmart Alerts"]
    end

    GH -->|Webhook| CI
    TRIVY -->|Push Image| ECR
    CI -->|Notify| N8N
    GH -->|Sync| CD
    SYNC --> K8S
    K8S --> OBS
    BE -->|SSL| RDS
    K8S ---|Secrets via IRSA| SM
    EKS --- K8S

    style DEV fill:#0d1117,stroke:#58a6ff,stroke-width:2px,color:#c9d1d9
    style INFRA fill:#0d1117,stroke:#f0883e,stroke-width:2px,color:#c9d1d9
    style CI fill:#0d1117,stroke:#d24939,stroke-width:2px,color:#c9d1d9
    style CD fill:#0d1117,stroke:#ef7b4d,stroke-width:2px,color:#c9d1d9
    style K8S fill:#0d1117,stroke:#326ce5,stroke-width:2px,color:#c9d1d9
    style OBS fill:#0d1117,stroke:#e6a817,stroke-width:2px,color:#c9d1d9
    style NOTIFY fill:#0d1117,stroke:#ff6d5a,stroke-width:2px,color:#c9d1d9
```

</div>

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                     REPOSITORY MAP                         -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 📦 Repository Map

<div align="center">

| Repository | Purpose | Key Technologies |
|:----------:|:--------|:----------------:|
| **[Terraform-Infra](https://github.com/NTI-Django-React-Project/Terraform-Infra)** | AWS infrastructure as code — EKS, RDS, VPC, IAM, ECR, Secrets Manager | ![Terraform](https://img.shields.io/badge/-Terraform-844FBA?style=flat-square&logo=terraform&logoColor=white) ![AWS](https://img.shields.io/badge/-AWS-FF9900?style=flat-square&logo=amazonaws&logoColor=white) |
| **[App-Back-End](https://github.com/NTI-Django-React-Project/App-Back-End)** | Django REST backend with full Jenkins CI/CD pipeline | ![Django](https://img.shields.io/badge/-Django-092E20?style=flat-square&logo=django&logoColor=white) ![Jenkins](https://img.shields.io/badge/-Jenkins-D24939?style=flat-square&logo=jenkins&logoColor=white) |
| **[App-Front-End](https://github.com/NTI-Django-React-Project/App-Front-End)** | React frontend served via Nginx with Jenkins CI/CD | ![React](https://img.shields.io/badge/-React-61DAFB?style=flat-square&logo=react&logoColor=black) ![Nginx](https://img.shields.io/badge/-Nginx-009639?style=flat-square&logo=nginx&logoColor=white) |
| **[k8s-manifests](https://github.com/NTI-Django-React-Project/k8s-manifests)** | Kubernetes manifests — Argo CD apps, deployments, observability, logging | ![K8s](https://img.shields.io/badge/-Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white) ![Argo](https://img.shields.io/badge/-ArgoCD-EF7B4D?style=flat-square&logo=argo&logoColor=white) |
| **[n8n](https://github.com/NTI-Django-React-Project/n8n)** | Workflow automation — smart CI/CD notifications via webhooks | ![n8n](https://img.shields.io/badge/-n8n-ff6d5a?style=flat-square&logo=n8n&logoColor=white) |

</div>

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                  INFRASTRUCTURE LAYER                      -->
<!-- ═══════════════════════════════════════════════════════════ -->

## ☁️ Infrastructure Layer — Terraform

> **Fully modular IaC** provisioning all AWS resources needed to run a production Kubernetes platform.

<div align="center">

```mermaid
%%{init: {'theme': 'dark'}}%%
flowchart LR
    subgraph TF_BACKEND["🗃️ Remote State"]
        S3["S3 Bucket"]
        DDB["DynamoDB Lock"]
    end

    subgraph MODULES["📦 Reusable Modules"]
        direction TB
        M_VPC["security-group\nsecurity-group-rule"]
        M_EKS["eks\neks-network\neks-namespace\neks-irsa"]
        M_IAM["iam-role · iam-policy\niam-group · iam-user"]
        M_DATA["rds\necr\nsecret-manager"]
        M_COMPUTE["ec2"]
    end

    subgraph STACKS["🏗️ Infrastructure Stacks"]
        EKS_STACK["EKS Infra\nmain.tf · 28 KB"]
        JENKINS_STACK["Stand-Alone\nJenkins Server"]
    end

    TF_BACKEND --> STACKS
    MODULES --> STACKS

    style TF_BACKEND fill:#0d1117,stroke:#844fba,stroke-width:2px,color:#c9d1d9
    style MODULES fill:#0d1117,stroke:#844fba,stroke-width:2px,color:#c9d1d9
    style STACKS fill:#0d1117,stroke:#844fba,stroke-width:2px,color:#c9d1d9
```

</div>

### 🧱 Module Inventory

<table>
<tr><th>Category</th><th>Modules</th><th>What They Provision</th></tr>
<tr>
<td><b>🌐 Networking</b></td>
<td><code>security-group</code> · <code>security-group-rule</code></td>
<td>VPC security groups with fine-grained ingress/egress rules</td>
</tr>
<tr>
<td><b>⎈ Kubernetes</b></td>
<td><code>eks</code> · <code>eks-network</code> · <code>eks-namespace</code> · <code>eks-irsa</code></td>
<td>EKS cluster, networking add-ons, namespace isolation, IAM Roles for Service Accounts</td>
</tr>
<tr>
<td><b>👤 Identity</b></td>
<td><code>iam-role</code> · <code>iam-policy</code> · <code>iam-group</code> · <code>iam-user</code> · <code>iam-role-policy-attachment</code> · <code>iam-group-policy-attachment</code> · <code>iam-user-group-membership</code></td>
<td>Complete IAM hierarchy — roles, policies, groups, users, and least-privilege bindings</td>
</tr>
<tr>
<td><b>🗄️ Data</b></td>
<td><code>rds</code> · <code>ecr</code> · <code>secret-manager</code></td>
<td>PostgreSQL RDS instance, ECR container repos, and AWS Secrets Manager entries</td>
</tr>
<tr>
<td><b>🖥️ Compute</b></td>
<td><code>ec2</code></td>
<td>Stand-alone Jenkins server on EC2</td>
</tr>
</table>

### 📐 Infrastructure Stacks

| Stack | Description |
|-------|-------------|
| **`eks infra/`** | The main production stack — provisions VPC, subnets, EKS cluster with managed node groups, RDS PostgreSQL, ECR repositories, Secrets Manager, IAM roles/IRSA, and all networking |
| **`stand-alone-jenkins-server/`** | Dedicated EC2 instance pre-configured as a Jenkins CI server with Docker, AWS CLI, and security tooling |
| **`terraform-backend/`** | S3 bucket + DynamoDB table for remote state management and state locking |

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                     CI/CD PIPELINES                        -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 🔄 CI/CD Pipelines — Jenkins

> **Declarative, security-first pipelines** that test, scan, build, and push container images — with zero-trust Docker builds via **Kaniko**.

<div align="center">

```mermaid
%%{init: {'theme': 'dark'}}%%
flowchart LR
    subgraph BACKEND_PIPE["🐍 Backend Pipeline — 14 Stages"]
        direction LR
        B1["📥 Checkout"] --> B2["🗄️ Start Test DB"] --> B3["🐍 Setup Python"] --> B4["🔌 Validate DB"] --> B5["📋 Migrations"] --> B6["🏗️ Build Django"] --> B7["✅ Tests + Coverage"]
        B7 --> B8["🔍 SonarQube"] --> B9["🚦 Quality Gate"] --> B10["🛡️ OWASP"] --> B11["📚 Nexus Upload"] --> B12["🐳 Kaniko Build"] --> B13["🔒 Trivy Scan"] --> B14["📤 Push to ECR"]
    end

    style BACKEND_PIPE fill:#0d1117,stroke:#d24939,stroke-width:2px,color:#c9d1d9
```

```mermaid
%%{init: {'theme': 'dark'}}%%
flowchart LR
    subgraph FRONTEND_PIPE["⚛️ Frontend Pipeline — 10 Stages"]
        direction LR
        F1["📥 Checkout"] --> F2["📦 npm install"] --> F3["🏗️ npm run build"] --> F4["🔍 SonarQube"] --> F5["🚦 Quality Gate"]
        F5 --> F6["🛡️ OWASP"] --> F7["📚 Nexus Upload"] --> F8["🐳 Kaniko Build"] --> F9["🔒 Trivy Image"] --> F10["🔍 Trivy FS"]
    end

    style FRONTEND_PIPE fill:#0d1117,stroke:#61dafb,stroke-width:2px,color:#c9d1d9
```

</div>

### 🔑 Pipeline Highlights

<table>
<tr>
<td width="50%">

**🐍 Backend Pipeline**
- Real PostgreSQL 15 container spun up for integration tests
- `pytest` with coverage reports (HTML, XML, terminal)
- Multi-stage Docker build → **non-root** `appuser`
- Images pushed to **AWS ECR** (`eu-north-1`)
- Tags: `{BUILD_NUMBER}-{SHORT_COMMIT}` + `latest`

</td>
<td width="50%">

**⚛️ Frontend Pipeline**
- React production build via Node.js 18
- Multi-stage Docker: `node:20-alpine` → `nginx:alpine`
- Nginx reverse-proxy config for `/api` → backend
- Build artifacts archived to **Nexus Raw** repository
- Images pushed to **Docker Hub**

</td>
</tr>
</table>

### 🛡️ Security Gates (Both Pipelines)

| Gate | Tool | Purpose |
|------|------|---------|
| **Code Quality** | SonarQube + Quality Gate | Static analysis, code smells, coverage enforcement — pipeline **aborts** on failure |
| **Dependency Scan** | OWASP Dependency-Check + NVD | CVE scanning for all project dependencies |
| **Image Scan** | Trivy | HIGH/CRITICAL vulnerability scanning on final container images |
| **Rootless Builds** | Kaniko | In-cluster, daemon-less Docker builds — no privileged containers needed |

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                    GITOPS & DEPLOYMENT                     -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 🚀 GitOps & Deployment — Argo CD

> **App-of-Apps pattern** with **sync waves** for ordered, self-healing deployments.

Argo CD watches the `k8s-manifests` repository and automatically reconciles the cluster state. Every component is declared as an Argo CD `Application` resource, organized by sync waves to respect dependency ordering.

<div align="center">

```mermaid
%%{init: {'theme': 'dark'}}%%
flowchart TB
    PLATFORM["🎯 Platform App\n(App-of-Apps)"]

    subgraph WAVE0["🌊 Wave 0 — Foundation"]
        ALB["AWS Load Balancer\nController"]
    end

    subgraph WAVE1["🌊 Wave 1 — CRDs"]
        CRDS["Prometheus CRDs"]
    end

    subgraph WAVE2["🌊 Wave 2 — Operators"]
        ESO["External Secrets\nOperator"]
    end

    subgraph WAVE3["🌊 Wave 3 — Platform Services"]
        MON["Monitoring\nPrometheus + Grafana"]
        MEM["Memcached"]
    end

    subgraph WAVE4["🌊 Wave 4 — Applications"]
        BK["Backend\nDjango + Redis"]
        LOG["Logging\nEFK Stack"]
    end

    subgraph WAVE5["🌊 Wave 5 — Frontend"]
        FT["Frontend\nReact + Nginx"]
    end

    PLATFORM --> WAVE0 --> WAVE1 --> WAVE2 --> WAVE3 --> WAVE4 --> WAVE5

    style PLATFORM fill:#ef7b4d,stroke:#333,stroke-width:2px,color:white
    style WAVE0 fill:#0d1117,stroke:#58a6ff,stroke-width:1px,color:#c9d1d9
    style WAVE1 fill:#0d1117,stroke:#58a6ff,stroke-width:1px,color:#c9d1d9
    style WAVE2 fill:#0d1117,stroke:#58a6ff,stroke-width:1px,color:#c9d1d9
    style WAVE3 fill:#0d1117,stroke:#58a6ff,stroke-width:1px,color:#c9d1d9
    style WAVE4 fill:#0d1117,stroke:#58a6ff,stroke-width:1px,color:#c9d1d9
    style WAVE5 fill:#0d1117,stroke:#58a6ff,stroke-width:1px,color:#c9d1d9
```

</div>

### ⎈ Kubernetes Resources Deployed

<table>
<tr>
<td width="50%">

**Backend Namespace**
- `Deployment` — 2 replicas, Gunicorn workers
- `Service` — ClusterIP on port 8000
- `Ingress` — ALB with SSL termination (`/api`)
- `ConfigMap` — RDS host, DB name, Redis URL
- `SecretStore` — AWS Secrets Manager via IRSA
- `ExternalSecret` — Auto-syncs RDS password
- `Redis` — In-cluster cache (Deployment + Service)
- `Job` — Database migration runner

</td>
<td width="50%">

**Frontend Namespace**
- `Deployment` — 2 replicas, Nginx serving React build
- `Service` — ClusterIP on port 80
- `Ingress` — ALB with SSL termination (`/`)

**Shared Infrastructure**
- `AWS ALB Controller` — Kubernetes Ingress → AWS ALB
- `External Secrets Operator` — K8s ↔ AWS Secrets Manager
- Shared ALB group (`shared-alb`) for cost optimization
- SSL via ACM certificate

</td>
</tr>
</table>

### 🔐 Secrets Management Flow

```mermaid
%%{init: {'theme': 'dark'}}%%
flowchart LR
    A["AWS Secrets Manager\n🔑 RDS Password"] -->|IRSA Auth| B["External Secrets\nOperator"]
    B -->|Creates/Syncs| C["K8s Secret\nrds-db-secret"]
    C -->|Mounted as env| D["Backend Pod\n🐍 Django"]
    
    E["ConfigMap\nrds-config"] -->|"DB_HOST, DB_NAME\nDB_USER, REDIS_URL"| D

    style A fill:#f0883e,stroke:#333,color:white
    style B fill:#326ce5,stroke:#333,color:white
    style C fill:#238636,stroke:#333,color:white
    style D fill:#0d1117,stroke:#58a6ff,color:#c9d1d9
    style E fill:#0d1117,stroke:#58a6ff,color:#c9d1d9
```

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                   OBSERVABILITY STACK                      -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 📊 Observability Stack

> **Full-stack monitoring** with Prometheus metrics collection and Grafana visualization, deployed via Helm through Argo CD.

<div align="center">

```mermaid
%%{init: {'theme': 'dark'}}%%
flowchart LR
    subgraph METRICS["📈 Metrics Collection"]
        NE["Node Exporter\nHost Metrics"]
        KSM["Kube State Metrics\nK8s Objects"]
        KUB["Kubelet\nPod Metrics"]
    end

    PROM["🔴 Prometheus\n1 Replica · 5Gi cap"]

    GRAF["📊 Grafana\nPersistent · 5Gi\n🌐 grafana.yassinabuelsheikh.store"]

    NE --> PROM
    KSM --> PROM
    KUB --> PROM
    PROM --> GRAF

    style METRICS fill:#0d1117,stroke:#e6a817,stroke-width:2px,color:#c9d1d9
    style PROM fill:#e6522c,stroke:#333,stroke-width:2px,color:white
    style GRAF fill:#f46800,stroke:#333,stroke-width:2px,color:white
```

</div>

| Component | Configuration |
|-----------|--------------|
| **Prometheus** | 1 replica, 3h retention, 200MB storage cap, resource-bounded (200m–2000m CPU, 1–5Gi RAM) |
| **Grafana** | Persistent storage (5Gi), admin creds from K8s Secret, exposed via ALB Ingress with SSL |
| **Kube State Metrics** | Enabled with resource limits, tracking all K8s object states |
| **Node Exporter** | DaemonSet on all nodes, lightweight host-level metrics |

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                   CENTRALIZED LOGGING                      -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 📝 Centralized Logging — EFK

> **Elasticsearch + Fluent Bit + Kibana** for aggregating, searching, and visualizing logs from all pods across the cluster.

<div align="center">

```mermaid
%%{init: {'theme': 'dark'}}%%
flowchart LR
    PODS["🐳 All Pods\n/var/log/pods"] -->|Tail Logs| FB["📤 Fluent Bit\nDaemonSet\nv2.2.2"]
    FB -->|Index: fluent-bit| ES["🔍 Elasticsearch\nSingle-node\nv8.11.1"]
    ES --> KB["📊 Kibana\nDashboards\nv8.11.1"]

    style PODS fill:#0d1117,stroke:#58a6ff,color:#c9d1d9
    style FB fill:#49bda5,stroke:#333,color:white
    style ES fill:#fed10a,stroke:#333,color:black
    style KB fill:#e8478b,stroke:#333,color:white
```

</div>

| Component | Details |
|-----------|---------|
| **Fluent Bit** | DaemonSet tailing `/var/log/pods/*/*/*.log`, forwarding to Elasticsearch on port 9200 |
| **Elasticsearch** | Single-node deployment, 2Gi memory, security disabled for internal use, readiness probe on `/_cluster/health` |
| **Kibana** | Connected to Elasticsearch, readiness probe on `/api/status`, resource limits: 512Mi–1Gi RAM |

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                     N8N AUTOMATION                         -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 🤖 Intelligent Automation — n8n

> **Smart CI/CD notification workflows** that bridge Jenkins pipelines with the engineering team through rich, classified alerts.

<div align="center">

```mermaid
%%{init: {'theme': 'dark'}}%%
flowchart LR
    J["🔄 Jenkins\nPipeline"] -->|"POST /webhook/jenkins-notify\nor /webhook/essam"| W["📡 n8n\nWebhook"]
    W --> P["⚙️ Data Processor\nClassify Failure Type"]
    
    P -->|"SUCCESS"| S_OK["✅ Success Report"]
    P -->|"BUILD_FAILED"| S_FAIL["❌ Build Report"]
    P -->|"SECURITY_ISSUE"| S_SEC["🛡️ Security Report\nTrivy + OWASP"]

    S_OK --> EMAIL["📧 Gmail\nRich HTML Email"]
    S_FAIL --> EMAIL
    S_SEC --> EMAIL

    style J fill:#d24939,stroke:#333,color:white
    style W fill:#ff6d5a,stroke:#333,color:white
    style P fill:#0d1117,stroke:#58a6ff,color:#c9d1d9
    style EMAIL fill:#ea4335,stroke:#333,color:white
```

</div>

### 📨 Notification Features

| Feature | Description |
|---------|-------------|
| **Dynamic Headers** | Color-coded — 🟢 green for success, 🔴 red for failure |
| **Failure Classification** | Distinguishes between `BUILD_FAILED` and `SECURITY_ISSUE` |
| **Security Insights** | Highlights Trivy critical CVEs and OWASP dependency vulnerabilities |
| **Code Quality State** | Includes SonarQube issue summary when available |
| **Build Metadata** | Job name, build number, image tag, and direct link to Jenkins build |

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                   SECURITY PRACTICES                       -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 🔐 Security Practices

<div align="center">

```mermaid
%%{init: {'theme': 'dark'}}%%
mindmap
  root((🔐 Security))
    🐳 Container Security
      Multi-stage Docker builds
      Non-root containers (appuser)
      Kaniko daemon-less builds
      Trivy image scanning
    🔍 Code Security
      SonarQube static analysis
      Quality Gate enforcement
      OWASP Dependency-Check
    ☁️ Cloud Security
      IAM least privilege
      IRSA for pod-level access
      Secrets Manager integration
      SSL/TLS via ACM
    ⎈ Kubernetes Security
      Namespace isolation
      NetworkPolicy for Memcached
      External Secrets Operator
      Service account bindings
```

</div>

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                    NETWORK TOPOLOGY                        -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 🌐 Traffic Flow & Network Topology

<div align="center">

```mermaid
%%{init: {'theme': 'dark'}}%%
flowchart TB
    USER["👤 User\nhttps://yassinabuelsheikh.store"] -->|HTTPS :443| ALB["⚖️ AWS ALB\nShared ALB Group\nSSL Termination"]
    
    ALB -->|"/ → frontend:80"| FE["⚛️ Frontend\nNginx · React"]
    ALB -->|"/api → backend:8000"| BE["🐍 Backend\nDjango · Gunicorn"]
    ALB -->|"grafana.* → grafana:3000"| GRAF["📊 Grafana"]

    FE -->|"Internal /api proxy"| BE
    BE -->|"SSL :5432"| RDS["🗄️ RDS PostgreSQL\ngig-route.*.rds.amazonaws.com"]
    BE -->|":6379"| REDIS["⚡ Redis"]

    style USER fill:#58a6ff,stroke:#333,color:white
    style ALB fill:#f0883e,stroke:#333,color:white
    style FE fill:#61dafb,stroke:#333,color:black
    style BE fill:#092e20,stroke:#333,color:white
    style RDS fill:#3b48cc,stroke:#333,color:white
    style REDIS fill:#dc382d,stroke:#333,color:white
    style GRAF fill:#f46800,stroke:#333,color:white
```

</div>

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                     TECH STACK                             -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 🧰 Technology Stack

<div align="center">

<table>
<tr><th>Layer</th><th>Technologies</th></tr>
<tr>
<td><b>Application</b></td>
<td>
<img src="https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white" alt="Django"/>
<img src="https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black" alt="React"/>
<img src="https://img.shields.io/badge/Nginx-009639?style=flat-square&logo=nginx&logoColor=white" alt="Nginx"/>
<img src="https://img.shields.io/badge/Gunicorn-499848?style=flat-square&logo=gunicorn&logoColor=white" alt="Gunicorn"/>
<img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL"/>
<img src="https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white" alt="Redis"/>
</td>
</tr>
<tr>
<td><b>Infrastructure</b></td>
<td>
<img src="https://img.shields.io/badge/Terraform-844FBA?style=flat-square&logo=terraform&logoColor=white" alt="Terraform"/>
<img src="https://img.shields.io/badge/AWS_EKS-FF9900?style=flat-square&logo=amazonaws&logoColor=white" alt="EKS"/>
<img src="https://img.shields.io/badge/AWS_RDS-527FFF?style=flat-square&logo=amazonaws&logoColor=white" alt="RDS"/>
<img src="https://img.shields.io/badge/AWS_ECR-FF9900?style=flat-square&logo=amazonaws&logoColor=white" alt="ECR"/>
</td>
</tr>
<tr>
<td><b>CI/CD</b></td>
<td>
<img src="https://img.shields.io/badge/Jenkins-D24939?style=flat-square&logo=jenkins&logoColor=white" alt="Jenkins"/>
<img src="https://img.shields.io/badge/ArgoCD-EF7B4D?style=flat-square&logo=argo&logoColor=white" alt="ArgoCD"/>
<img src="https://img.shields.io/badge/Kaniko-FFA600?style=flat-square&logo=google&logoColor=white" alt="Kaniko"/>
<img src="https://img.shields.io/badge/Nexus-1B1C30?style=flat-square&logo=sonatype&logoColor=white" alt="Nexus"/>
</td>
</tr>
<tr>
<td><b>Security</b></td>
<td>
<img src="https://img.shields.io/badge/SonarQube-4E9BCD?style=flat-square&logo=sonarqube&logoColor=white" alt="SonarQube"/>
<img src="https://img.shields.io/badge/Trivy-1904DA?style=flat-square&logo=aqua&logoColor=white" alt="Trivy"/>
<img src="https://img.shields.io/badge/OWASP-000000?style=flat-square&logo=owasp&logoColor=white" alt="OWASP"/>
</td>
</tr>
<tr>
<td><b>Observability</b></td>
<td>
<img src="https://img.shields.io/badge/Prometheus-E6522C?style=flat-square&logo=prometheus&logoColor=white" alt="Prometheus"/>
<img src="https://img.shields.io/badge/Grafana-F46800?style=flat-square&logo=grafana&logoColor=white" alt="Grafana"/>
<img src="https://img.shields.io/badge/Elasticsearch-005571?style=flat-square&logo=elasticsearch&logoColor=white" alt="ES"/>
<img src="https://img.shields.io/badge/Fluent_Bit-49BDA5?style=flat-square&logo=fluentbit&logoColor=white" alt="Fluent Bit"/>
<img src="https://img.shields.io/badge/Kibana-E8478B?style=flat-square&logo=kibana&logoColor=white" alt="Kibana"/>
</td>
</tr>
<tr>
<td><b>Automation</b></td>
<td>
<img src="https://img.shields.io/badge/n8n-ff6d5a?style=flat-square&logo=n8n&logoColor=white" alt="n8n"/>
</td>
</tr>
</table>

</div>

---

<!-- ═══════════════════════════════════════════════════════════ -->
<!--                    TEAM & PURPOSE                          -->
<!-- ═══════════════════════════════════════════════════════════ -->

## 👥 Team & Purpose

<div align="center">

> 🎓 This organization serves as our **DevOps engineering practice lab** — a hands-on environment where we design, build, and operate production-grade infrastructure and CI/CD systems using real-world tools and best practices.

</div>

### 🎯 What We Practice

<table>
<tr>
<td align="center" width="20%">

**☁️**<br/>
**IaC**<br/>
<sub>Terraform modules,<br/>remote state, AWS</sub>

</td>
<td align="center" width="20%">

**🔄**<br/>
**CI/CD**<br/>
<sub>Jenkins declarative<br/>pipelines, security gates</sub>

</td>
<td align="center" width="20%">

**⎈**<br/>
**Kubernetes**<br/>
<sub>EKS, namespaces,<br/>IRSA, ingress</sub>

</td>
<td align="center" width="20%">

**🚀**<br/>
**GitOps**<br/>
<sub>Argo CD, app-of-apps,<br/>sync waves</sub>

</td>
<td align="center" width="20%">

**📊**<br/>
**Observability**<br/>
<sub>Prometheus, Grafana,<br/>EFK stack</sub>

</td>
</tr>
</table>

---

## 📄 License

This project is maintained for educational purposes as part of the NTI DevOps training program.

---

## 👥 Our Team & Contributions

<p align="center">
  <strong>Meet the DevOps Engineers behind this project</strong>
</p>

<table align="center">
  <tr>
    <td align="center" width="150px">
      <a href="https://github.com/Abdelaziz-Ak">
        <img src="https://github.com/Abdelaziz-Ak.png" width="100px;" style="border-radius: 50%;" alt="Abdelaziz Ak"/>
        <br />
        <sub><b>Abdelaziz Ak</b></sub>
      </a>
      <br />
      <sub>Infrastructure & Terraform</sub>
      <br />
      <br />
      <img src="https://img.shields.io/badge/Commits-150+-brightgreen" alt="Commits"/>
    </td>
    <td align="center" width="150px">
      <a href="https://github.com/Yassin-Abuelsheikh">
        <img src="https://github.com/Yassin-Abuelsheikh.png" width="100px;" style="border-radius: 50%;" alt="Yassin Abu El-Sheikh"/>
        <br />
        <sub><b>Yassin Abu El-Sheikh</b></sub>
      </a>
      <br />
      <sub>Kubernetes & EKS</sub>
      <br />
      <br />
      <img src="https://img.shields.io/badge/Commits-150+-brightgreen" alt="Commits"/>
    </td>
    <td align="center" width="150px">
      <a href="https://github.com/moessam634">
        <img src="https://github.com/moessam634.png" width="100px;" style="border-radius: 50%;" alt="Mohamed Essam"/>
        <br />
        <sub><b>Mohamed Essam</b></sub>
      </a>
      <br />
      <sub>CI Pipelines</sub>
      <br />
      <br />
      <img src="https://img.shields.io/badge/Commits-150+-brightgreen" alt="Commits"/>
    </td>
  </tr>
  <tr>
    <td align="center" width="150px">
      <a href="https://github.com/mazenmostafa001">
        <img src="https://github.com/mazenmostafa001.png" width="100px;" style="border-radius: 50%;" alt="Name 4"/>
        <br />
        <sub><b>Mazen Mostafa</b></sub>
      </a>
      <br />
      <sub>CD Pipelines</sub>
      <br />
      <br />
      <img src="https://img.shields.io/badge/Commits-150+-brightgreen" alt="Commits"/>
    </td>
    <td align="center" width="150px">
      <a href="https://github.com/abdo073">
        <img src="https://github.com/abdo073.png" width="100px;" style="border-radius: 50%;" alt="Abdulrahman Mahmoud"/>
        <br />
        <sub><b>Abdulrahman Mahmoud</b></sub>
      </a>
      <br />
      <sub>EFK Monitoring</sub>
      <br />
      <br />
      <img src="https://img.shields.io/badge/Commits-150+-brightgreen" alt="Commits"/>
    </td>
    <td align="center" width="150px">
      <br />
      <br />
      <sub><b>🤝 Collaboration</b></sub>
      <br />
      <sub>Working together</sub>
      <br />
      <sub>Learning together</sub>
      <br />
      <sub>Growing together</sub>
    </td>
  </tr>
</table>


---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:161b22,100:1f6feb&height=120&section=footer" width="100%"/>

<sub>Built with ❤️ by the <b>NTI DevOps Gig Router Project Team</b> · DevOps Engineering Practice</sub>

</div>
