---
name: aspnet-core-devops
description: Master ASP.NET Core deployment, Docker, Azure cloud, CI/CD pipelines, and production infrastructure for enterprise applications.
---

# ASP.NET Core DevOps & Production

## DevOps Skills

### Docker & Containerization
- Dockerfile creation
- Multi-stage builds
- Image optimization
- Image layering
- Docker Compose
- Container networking
- Volume management
- Container security

### Azure Cloud Services
- App Service deployment
- Azure SQL Database
- Container Registry (ACR)
- Key Vault secrets
- Application Insights
- Log Analytics
- Azure Container Instances
- Azure Kubernetes Service (AKS)

### CI/CD Pipelines
- GitHub Actions
- Azure Pipelines
- Build automation
- Test automation
- Release pipelines
- Artifact management
- Environment promotion
- Rollback strategies

### Kubernetes & Orchestration
- Pod deployment
- Services and networking
- ConfigMaps and Secrets
- StatefulSets
- DaemonSets
- Horizontal Pod Autoscaler
- Helm charts
- GitOps with ArgoCD

### Infrastructure as Code
- Terraform
- ARM templates
- Bicep language
- Resource Group management
- Network configuration
- Auto-scaling setup

### Monitoring & Logging
- Application Insights setup
- Custom metrics
- Log aggregation
- Alert rules
- Dashboard creation
- Performance monitoring
- Error tracking
- Health checks

### Security in Production
- SSL/TLS certificates
- Secrets management
- Firewall configuration
- Network security
- DDoS protection
- Vulnerability scanning
- Compliance (GDPR, SOC 2)

## Deployment Examples

### Dockerfile
```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["MyApp.csproj", "."]
RUN dotnet restore
COPY . .
RUN dotnet build -c Release -o /app/build

FROM build AS publish
RUN dotnet publish -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

### GitHub Actions Workflow
```yaml
name: Build and Deploy

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
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
      run: dotnet build --no-restore --configuration Release

    - name: Test
      run: dotnet test --no-build --configuration Release

    - name: Publish
      run: dotnet publish -c Release -o ${{env.DOTNET_ROOT}}/myapp

    - name: Deploy to Azure
      uses: azure/appservice-deploy@v2
      with:
        app-name: my-app
        publish-profile: ${{ secrets.AZURE_PUBLISH_PROFILE }}
        package: ${{env.DOTNET_ROOT}}/myapp
```

### Kubernetes Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myregistry.azurecr.io/myapp:1.0
        ports:
        - containerPort: 80
        env:
        - name: ASPNETCORE_ENVIRONMENT
          value: "Production"
        livenessProbe:
          httpGet:
            path: /health
            port: 80
          initialDelaySeconds: 10
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /health/ready
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
```

### Azure Terraform
```hcl
resource "azurerm_app_service" "myapp" {
  name                = "myapp-${random_string.fqdn.result}"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  app_service_plan_id = azurerm_app_service_plan.plan.id

  site_config {
    dotnet_framework_version = "v8.0"
  }

  app_settings = {
    "WEBSITES_ENABLE_APP_SERVICE_STORAGE" = "false"
  }
}
```

## Assessment Criteria

- [ ] Create optimized Docker images
- [ ] Deploy to cloud platforms (Azure/AWS)
- [ ] Setup complete CI/CD pipelines
- [ ] Configure monitoring and alerts
- [ ] Manage secrets and configuration
- [ ] Implement auto-scaling
- [ ] Setup health checks
- [ ] Configure networking and security
- [ ] Implement disaster recovery
- [ ] Monitor application performance

