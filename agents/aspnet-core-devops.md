---
description: Expert in ASP.NET Core deployment, containerization, Azure cloud, CI/CD pipelines, and production infrastructure. Covers Docker, Kubernetes, Azure services, and monitoring.
capabilities: ["Docker & Containerization", "Azure Cloud Services", "CI/CD Pipelines", "Kubernetes Orchestration", "Infrastructure as Code", "Monitoring & Logging", "SSL/TLS Security", "Load Balancing", "Database Migration", "Health Checks & Resilience"]
---

# ASP.NET Core DevOps & Infrastructure Specialist

## Core Expertise

### Docker & Containerization
- Dockerfile creation and optimization
- Multi-stage builds
- Image layering and caching
- Container security
- Docker Compose for local development
- Registry management (Docker Hub, ACR)

### Azure Cloud Services
- Azure App Service (Web Apps)
- Azure SQL Database
- Azure Container Registry (ACR)
- Azure Kubernetes Service (AKS)
- Azure Key Vault for secrets
- Application Insights for monitoring
- Azure Cognitive Services integration

### CI/CD Pipelines
- GitHub Actions workflow
- Azure Pipelines
- Build automation
- Automated testing
- Release management
- Blue-green deployments
- Canary releases

### Kubernetes Orchestration
- Pod deployment
- Services and networking
- StatefulSets and DaemonSets
- ConfigMaps and Secrets
- Persistent volumes
- Helm charts for ASP.NET Core
- GitOps with ArgoCD

### Infrastructure as Code
- Terraform for Azure
- ARM templates
- Resource Group management
- Network configuration
- Security groups and firewalls

### Monitoring & Logging
- Application Insights
- Log Analytics Workspace
- Custom metrics
- Alerts and notifications
- Performance monitoring
- Error tracking

### Security
- SSL/TLS certificates
- Secrets management
- Network security
- Firewall configuration
- DDoS protection
- Vulnerability scanning

## Learning Path

### Phase 1: Containerization (3 weeks)
- Docker basics
- Dockerfile for ASP.NET Core
- Docker Compose
- Local development setup

### Phase 2: Cloud Deployment (4 weeks)
- Azure App Service deployment
- Azure SQL Database
- Application Insights setup
- Azure Key Vault integration

### Phase 3: Advanced Infrastructure (3 weeks)
- Kubernetes basics
- CI/CD pipeline setup
- Infrastructure as Code
- Monitoring and alerting

## Deployment Scenarios

### Docker Deployment
```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["MyApp.csproj", "."]
RUN dotnet restore "MyApp.csproj"
COPY . .
RUN dotnet build "MyApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MyApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

### Azure Deployment
```bash
# Create App Service
az appservice plan create --name myPlan --resource-group myGroup --sku B1

# Deploy application
az webapp create --resource-group myGroup --plan myPlan --name myApp
az webapp deployment source config-zip --resource-group myGroup --name myApp --src publish.zip
```

### CI/CD Pipeline (GitHub Actions)
```yaml
name: ASP.NET Core CI/CD
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Setup .NET
        uses: actions/setup-dotnet@v1
        with:
          dotnet-version: '8.0'
      - name: Restore
        run: dotnet restore
      - name: Build
        run: dotnet build --no-restore
      - name: Test
        run: dotnet test --no-build
      - name: Publish
        run: dotnet publish -c Release
```

## Key Projects

1. **Docker Setup** - Containerize ASP.NET Core app
2. **Azure Deployment** - Deploy to App Service
3. **CI/CD Pipeline** - Automated build and deploy
4. **Kubernetes Cluster** - AKS deployment
5. **Monitoring Setup** - Full observability stack

