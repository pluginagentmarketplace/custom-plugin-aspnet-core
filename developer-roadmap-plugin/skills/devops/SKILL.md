---
name: devops-cloud-skills
description: Master DevOps and Cloud Infrastructure including containerization, orchestration, CI/CD, Infrastructure as Code, and cloud platforms.
---

# DevOps & Cloud Infrastructure Skills

## DevOps Fundamentals

### 5 Core Pillars
1. **Infrastructure:** Servers, networking, storage
2. **Containerization:** Docker, images, registries
3. **Orchestration:** Kubernetes, service management
4. **Automation:** IaC, CI/CD pipelines
5. **Monitoring:** Observability, alerting

## Learning Path

### Phase 1: Linux & Scripting (Weeks 1-4)
- Linux commands and navigation
- Bash scripting fundamentals
- User and permission management
- Package management (apt, yum)

### Phase 2: Containerization (Weeks 5-8)
- Docker basics
- Dockerfile creation
- Image management
- Docker Compose

### Phase 3: Orchestration (Weeks 9-12)
- Kubernetes architecture
- Pods, Services, Deployments
- ConfigMaps and Secrets
- Helm charts

### Phase 4: Infrastructure as Code (Weeks 13-16)
- Terraform basics
- AWS resource provisioning
- State management
- Modules and reusability

### Phase 5: CI/CD & Monitoring (Weeks 17+)
- GitHub Actions / GitLab CI
- Automated testing
- Deployment strategies
- Prometheus & Grafana

## Docker Workflow

```
Dockerfile
    ↓
docker build
    ↓
Docker Image
    ↓
docker push
    ↓
Docker Registry (Docker Hub, ECR)
    ↓
docker run / kubernetes deploy
    ↓
Running Container
```

## Kubernetes Architecture

### Core Components
- **API Server:** Cluster control
- **Kubelet:** Node agent
- **Scheduler:** Pod placement
- **Controller Manager:** Desired state
- **etcd:** Data storage

### Resources
- **Pod:** Smallest deployable unit
- **Service:** Network abstraction
- **Deployment:** Replicated pods
- **StatefulSet:** Stateful applications
- **Ingress:** External access

## Infrastructure as Code

### Terraform Structure
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}

output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

## CI/CD Pipeline

```
Git Push
  ↓ (webhook)
Run Tests
  ↓ (if pass)
Build Artifact
  ↓
Push to Registry
  ↓
Deploy to Staging
  ↓ (smoke tests)
Deploy to Production
  ↓
Monitor & Alert
```

## Cloud Platform Essentials

### AWS Services
- EC2 (Compute)
- S3 (Storage)
- RDS (Databases)
- Lambda (Serverless)
- CloudFormation (IaC)
- ALB (Load Balancer)

### GCP Services
- Compute Engine
- Cloud Storage
- Cloud SQL
- Cloud Functions
- Terraform support

### Azure Services
- Virtual Machines
- App Service
- SQL Database
- Functions
- ARM Templates

## Monitoring Stack

### Prometheus
- Metric collection
- Scrape targets
- Time-series database

### Grafana
- Visualization
- Dashboards
- Alerting rules

### Logging (ELK)
- Elasticsearch: Storage
- Logstash: Processing
- Kibana: Visualization

## Key Competencies

1. **Linux Administration** - System management
2. **Docker** - Containerization
3. **Kubernetes** - Orchestration
4. **Terraform** - IaC
5. **CI/CD** - Automation
6. **AWS/Cloud** - Cloud platform
7. **Monitoring** - Observability
8. **Networking** - DNS, VPC, security
9. **Bash Scripting** - Automation
10. **Git** - Version control

