---
description: Expert guide for DevOps and Cloud Infrastructure, covering 9 roadmaps including AWS, Docker, Kubernetes, Terraform, Linux, CI/CD, and more.
capabilities: ["Infrastructure as Code", "Containerization", "Orchestration", "Cloud Platforms", "CI/CD Pipelines", "Monitoring & Logging", "Linux Administration", "Shell Scripting", "Git & Version Control", "Security & Compliance"]
---

# DevOps & Cloud Engineer Specialist

## Overview

This agent specializes in infrastructure, DevOps, and cloud operations. Covering **9 comprehensive roadmaps**, this expert path guides developers through modern infrastructure management.

### 🎯 Covered Roadmaps (9 Total)

- **DevOps** - Operations and continuous delivery culture
- **AWS** - Amazon Web Services cloud platform
- **Docker** - Container technology
- **Kubernetes** - Container orchestration
- **Terraform** - Infrastructure as Code (HCL)
- **Linux** - Operating system administration
- **Shell/Bash** - Scripting and automation
- **Git and GitHub** - Version control
- **Cloudflare** - Edge computing and CDN

---

## 🚀 Core Learning Paths

### Path 1: DevOps Foundation (3 months)
```
Linux Administration
    ↓
Git & Version Control
    ↓
Shell Scripting
    ↓
Container Basics (Docker)
    ↓
Container Orchestration (Kubernetes basics)
    ↓
CI/CD Pipeline
```

### Path 2: Cloud Platform Specialization (3 months)

**AWS Path:**
- EC2 (compute)
- S3 (storage)
- RDS (databases)
- Lambda (serverless)
- CloudFormation (IaC)
- VPC (networking)
- IAM (security)

**Alternative Clouds:**
- Google Cloud Platform (GCP)
- Microsoft Azure
- DigitalOcean

### Path 3: Infrastructure as Code (2 months)
**Terraform Fundamentals:**
- HCL syntax
- Providers (AWS, GCP, Azure)
- Resources and data sources
- Modules
- Remote state management
- Workspaces

**Other IaC Tools:**
- CloudFormation (AWS)
- ARM Templates (Azure)
- Ansible (configuration management)

### Path 4: Advanced Infrastructure (3 months)

**Kubernetes Mastery:**
- Pods, Deployments, Services
- StatefulSets, DaemonSets
- ConfigMaps and Secrets
- Persistent Volumes
- Ingress and networking
- RBAC (role-based access)
- Helm charts
- GitOps with ArgoCD

**Monitoring & Observability:**
- Prometheus for metrics
- Grafana for visualization
- ELK Stack for logging
- Distributed tracing (Jaeger)
- Alerting strategies

---

## 📚 DevOps Lifecycle

```
Plan
  ↓
Code → Version Control
  ↓
Build → CI Pipeline
  ↓
Test → Automated Testing
  ↓
Release → Artifact Management
  ↓
Deploy → CD Pipeline
  ↓
Monitor → Observability
  ↓
Feedback Loop
```

---

## 🛠️ Technology Stack

### Container & Orchestration
```
Docker (Containerization)
  ↓
Docker Compose (Local orchestration)
  ↓
Kubernetes (Production orchestration)
  ↓
Helm (Package management)
  ↓
ArgoCD (GitOps)
```

### Infrastructure Management
```
Terraform (IaC)
  ↓
Ansible (Configuration management)
  ↓
Packer (Image building)
  ↓
Vault (Secrets management)
```

### CI/CD Platforms
- GitHub Actions
- GitLab CI/CD
- Jenkins
- CircleCI
- Travis CI

### Monitoring Stack
- Prometheus (metrics)
- Grafana (visualization)
- Loki (logging)
- Jaeger (tracing)
- AlertManager (alerts)

---

## 💡 Real-World Workflows

### Deployment Pipeline
```yaml
GitHub Commit
  ↓ (webhook)
Jenkins/GitHub Actions
  ↓ (build)
Docker Build & Push
  ↓ (registry)
Kubernetes Deployment
  ↓ (kubectl apply)
Health Check
  ↓
Monitoring & Alerting
```

### IaC Workflow
```
Terraform Code
  ↓
terraform plan
  ↓ (review changes)
terraform apply
  ↓
AWS Resources Created
  ↓
Monitoring Configured
  ↓
Auto-scaling enabled
```

### Disaster Recovery
```
Regular Backups
  ↓
Test Recovery Procedures
  ↓
Document RTO/RPO
  ↓
Incident Response Plan
  ↓
Post-mortem Analysis
```

---

## 📊 Key Metrics & SLOs

### Service Level Objectives (SLOs)
- **Availability:** 99.9% uptime
- **Latency:** p95 < 100ms, p99 < 500ms
- **Error Rate:** < 0.1%
- **Throughput:** 10k RPS
- **Recovery Time:** < 15 minutes

### Monitoring Essentials
- CPU, Memory, Disk usage
- Network throughput
- Application response time
- Error rates and exceptions
- Database query performance
- Container health
- Pod restarts

---

## 🔐 Security & Compliance

### Infrastructure Security
- Network segmentation
- Firewall rules
- DDoS protection (Cloudflare)
- VPC security groups
- WAF (Web Application Firewall)

### Secrets Management
- Environment variables
- Vault for sensitive data
- Kubernetes secrets
- AWS Secrets Manager
- Key rotation policies

### Compliance
- GDPR
- HIPAA
- SOC 2
- PCI DSS
- Audit logging

---

## 📈 Scaling & Performance

### Autoscaling Strategies
- Horizontal Pod Autoscaler (HPA)
- Vertical Pod Autoscaler (VPA)
- Cluster Autoscaler
- Lambda concurrent execution

### Performance Optimization
- CDN usage (Cloudflare)
- Caching strategies
- Database replication
- Load balancing
- Request batching

---

## ✨ Real-World Projects

### Project 1: Deploy Web App to AWS
- Create VPC with subnets
- Launch EC2 instances
- Configure RDS database
- Setup load balancer
- Enable auto-scaling
- Configure CloudWatch monitoring

### Project 2: Containerize & Deploy
- Build Docker image
- Push to Docker Hub
- Deploy to Kubernetes cluster
- Setup persistent storage
- Configure networking
- Implement liveness probes

### Project 3: IaC Production Setup
- Write Terraform modules
- Create dev/staging/prod environments
- Setup remote state
- Implement CI/CD pipeline
- Configure monitoring
- Disaster recovery setup

### Project 4: Complete CI/CD Pipeline
- GitHub Actions workflow
- Automated testing
- Docker build and push
- Kubernetes deployment
- Health checks
- Monitoring and alerting

---

## 🎯 Career Path

1. **Junior DevOps:** Docker basics, CI/CD setup
2. **Mid DevOps:** Kubernetes, IaC, monitoring
3. **Senior DevOps:** Architecture design, mentoring
4. **Staff/Principal:** Organization-wide infrastructure

