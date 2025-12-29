# ASP.NET Core DevOps & Production Guide

## Overview

This guide covers deployment, containerization, CI/CD pipelines, and production infrastructure for ASP.NET Core applications.

## Table of Contents

1. [Docker & Containerization](#docker--containerization)
2. [CI/CD Pipelines](#cicd-pipelines)
3. [Azure Deployment](#azure-deployment)
4. [Kubernetes Orchestration](#kubernetes-orchestration)
5. [Monitoring & Observability](#monitoring--observability)
6. [Security Best Practices](#security-best-practices)

---

## Docker & Containerization

### Optimized Multi-Stage Dockerfile

```dockerfile
# Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy csproj and restore dependencies (layer caching)
COPY ["src/MyApp.Api/MyApp.Api.csproj", "MyApp.Api/"]
COPY ["src/MyApp.Domain/MyApp.Domain.csproj", "MyApp.Domain/"]
COPY ["src/MyApp.Infrastructure/MyApp.Infrastructure.csproj", "MyApp.Infrastructure/"]
RUN dotnet restore "MyApp.Api/MyApp.Api.csproj"

# Copy everything and build
COPY src/ .
RUN dotnet build "MyApp.Api/MyApp.Api.csproj" -c Release -o /app/build

# Publish stage
FROM build AS publish
RUN dotnet publish "MyApp.Api/MyApp.Api.csproj" -c Release -o /app/publish \
    --no-restore \
    /p:UseAppHost=false

# Runtime stage (smallest image)
FROM mcr.microsoft.com/dotnet/aspnet:8.0-alpine AS final
WORKDIR /app

# Security: Run as non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Copy published files
COPY --from=publish /app/publish .

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:80/health || exit 1

EXPOSE 80
ENTRYPOINT ["dotnet", "MyApp.Api.dll"]
```

### Docker Compose for Development

```yaml
version: '3.8'

services:
  api:
    build:
      context: .
      dockerfile: src/MyApp.Api/Dockerfile
    ports:
      - "5000:80"
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ConnectionStrings__DefaultConnection=Server=db;Database=MyApp;User=sa;Password=YourStrong@Passw0rd;TrustServerCertificate=True
    depends_on:
      db:
        condition: service_healthy
    networks:
      - myapp-network

  db:
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment:
      - ACCEPT_EULA=Y
      - SA_PASSWORD=YourStrong@Passw0rd
    ports:
      - "1433:1433"
    volumes:
      - sqlserver-data:/var/opt/mssql
    healthcheck:
      test: /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P "YourStrong@Passw0rd" -Q "SELECT 1"
      interval: 10s
      timeout: 3s
      retries: 10
    networks:
      - myapp-network

  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data
    networks:
      - myapp-network

volumes:
  sqlserver-data:
  redis-data:

networks:
  myapp-network:
    driver: bridge
```

---

## CI/CD Pipelines

### GitHub Actions Workflow

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  DOTNET_VERSION: '8.0.x'
  REGISTRY: myregistry.azurecr.io
  IMAGE_NAME: myapp-api

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4

    - name: Setup .NET
      uses: actions/setup-dotnet@v4
      with:
        dotnet-version: ${{ env.DOTNET_VERSION }}

    - name: Restore dependencies
      run: dotnet restore

    - name: Build
      run: dotnet build --no-restore --configuration Release

    - name: Test
      run: dotnet test --no-build --configuration Release --collect:"XPlat Code Coverage" --results-directory ./coverage

    - name: Upload coverage
      uses: codecov/codecov-action@v3
      with:
        directory: ./coverage

  security-scan:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4

    - name: Run Snyk to check for vulnerabilities
      uses: snyk/actions/dotnet@master
      env:
        SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}

  build-and-push:
    needs: [build-and-test, security-scan]
    runs-on: ubuntu-latest
    if: github.event_name == 'push' && github.ref == 'refs/heads/main'
    steps:
    - uses: actions/checkout@v4

    - name: Login to ACR
      uses: azure/docker-login@v1
      with:
        login-server: ${{ env.REGISTRY }}
        username: ${{ secrets.ACR_USERNAME }}
        password: ${{ secrets.ACR_PASSWORD }}

    - name: Build and push Docker image
      uses: docker/build-push-action@v5
      with:
        context: .
        push: true
        tags: |
          ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
          ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:latest
        cache-from: type=gha
        cache-to: type=gha,mode=max

  deploy:
    needs: build-and-push
    runs-on: ubuntu-latest
    environment: production
    steps:
    - uses: actions/checkout@v4

    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}

    - name: Deploy to AKS
      uses: azure/k8s-deploy@v4
      with:
        namespace: myapp
        manifests: |
          k8s/deployment.yaml
          k8s/service.yaml
        images: |
          ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
```

### Azure DevOps Pipeline

```yaml
trigger:
  branches:
    include:
    - main
    - develop

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'
  dotnetVersion: '8.0.x'
  containerRegistry: 'myregistry.azurecr.io'
  imageName: 'myapp-api'

stages:
- stage: Build
  jobs:
  - job: BuildAndTest
    steps:
    - task: UseDotNet@2
      inputs:
        packageType: 'sdk'
        version: $(dotnetVersion)

    - task: DotNetCoreCLI@2
      displayName: 'Restore'
      inputs:
        command: 'restore'

    - task: DotNetCoreCLI@2
      displayName: 'Build'
      inputs:
        command: 'build'
        arguments: '--configuration $(buildConfiguration) --no-restore'

    - task: DotNetCoreCLI@2
      displayName: 'Test'
      inputs:
        command: 'test'
        arguments: '--configuration $(buildConfiguration) --collect:"XPlat Code Coverage"'

    - task: PublishCodeCoverageResults@2
      inputs:
        summaryFileLocation: '$(Agent.TempDirectory)/**/coverage.cobertura.xml'

- stage: Docker
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
  jobs:
  - job: BuildPushImage
    steps:
    - task: Docker@2
      inputs:
        containerRegistry: 'ACR-Connection'
        repository: $(imageName)
        command: 'buildAndPush'
        Dockerfile: '**/Dockerfile'
        tags: |
          $(Build.BuildId)
          latest

- stage: Deploy
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
  jobs:
  - deployment: DeployToAKS
    environment: 'production'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: KubernetesManifest@0
            inputs:
              action: 'deploy'
              kubernetesServiceConnection: 'AKS-Connection'
              namespace: 'myapp'
              manifests: 'k8s/*.yaml'
              containers: '$(containerRegistry)/$(imageName):$(Build.BuildId)'
```

---

## Azure Deployment

### Infrastructure with Terraform

```hcl
# main.tf
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~>3.0"
    }
  }
  backend "azurerm" {
    resource_group_name  = "terraform-state-rg"
    storage_account_name = "tfstateaccount"
    container_name       = "tfstate"
    key                  = "myapp.tfstate"
  }
}

provider "azurerm" {
  features {}
}

# Resource Group
resource "azurerm_resource_group" "main" {
  name     = "myapp-${var.environment}-rg"
  location = var.location
  tags     = var.tags
}

# App Service Plan
resource "azurerm_service_plan" "main" {
  name                = "myapp-${var.environment}-plan"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  os_type             = "Linux"
  sku_name            = var.app_service_sku
}

# App Service
resource "azurerm_linux_web_app" "main" {
  name                = "myapp-${var.environment}-api"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  service_plan_id     = azurerm_service_plan.main.id

  site_config {
    application_stack {
      dotnet_version = "8.0"
    }
    health_check_path = "/health"
    always_on         = true
  }

  app_settings = {
    "ASPNETCORE_ENVIRONMENT"                = var.environment
    "ApplicationInsights__ConnectionString" = azurerm_application_insights.main.connection_string
  }

  identity {
    type = "SystemAssigned"
  }
}

# SQL Database
resource "azurerm_mssql_server" "main" {
  name                         = "myapp-${var.environment}-sql"
  resource_group_name          = azurerm_resource_group.main.name
  location                     = azurerm_resource_group.main.location
  version                      = "12.0"
  administrator_login          = var.sql_admin_login
  administrator_login_password = var.sql_admin_password

  azuread_administrator {
    login_username = "AzureAD Admin"
    object_id      = data.azurerm_client_config.current.object_id
  }
}

resource "azurerm_mssql_database" "main" {
  name           = "myapp"
  server_id      = azurerm_mssql_server.main.id
  sku_name       = var.sql_sku
  zone_redundant = var.environment == "production"
}

# Key Vault
resource "azurerm_key_vault" "main" {
  name                = "myapp-${var.environment}-kv"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  tenant_id           = data.azurerm_client_config.current.tenant_id
  sku_name            = "standard"

  purge_protection_enabled = true
}

# Application Insights
resource "azurerm_application_insights" "main" {
  name                = "myapp-${var.environment}-ai"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  application_type    = "web"
}

# Container Registry
resource "azurerm_container_registry" "main" {
  name                = "myapp${var.environment}acr"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  sku                 = "Standard"
  admin_enabled       = true
}
```

---

## Kubernetes Orchestration

### Health Checks in ASP.NET Core

```csharp
// Program.cs
builder.Services.AddHealthChecks()
    .AddCheck("self", () => HealthCheckResult.Healthy(), tags: new[] { "live" })
    .AddSqlServer(
        connectionString: builder.Configuration.GetConnectionString("DefaultConnection")!,
        name: "database",
        tags: new[] { "ready" })
    .AddRedis(
        redisConnectionString: builder.Configuration.GetConnectionString("Redis")!,
        name: "redis",
        tags: new[] { "ready" });

var app = builder.Build();

// Health endpoints
app.MapHealthChecks("/health/live", new HealthCheckOptions
{
    Predicate = check => check.Tags.Contains("live")
});

app.MapHealthChecks("/health/ready", new HealthCheckOptions
{
    Predicate = check => check.Tags.Contains("ready")
});

app.MapHealthChecks("/health/startup", new HealthCheckOptions
{
    Predicate = _ => true
});
```

### Helm Chart Structure

```yaml
# Chart.yaml
apiVersion: v2
name: aspnetcore-api
description: ASP.NET Core API Helm chart
version: 1.0.0
appVersion: "1.0.0"

# values.yaml
replicaCount: 3

image:
  repository: myregistry.azurecr.io/aspnetcore-api
  pullPolicy: Always
  tag: "latest"

service:
  type: ClusterIP
  port: 80

ingress:
  enabled: true
  className: nginx
  hosts:
    - host: api.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: api-tls
      hosts:
        - api.example.com

resources:
  limits:
    cpu: 500m
    memory: 512Mi
  requests:
    cpu: 250m
    memory: 256Mi

autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 10
  targetCPUUtilizationPercentage: 70

env:
  ASPNETCORE_ENVIRONMENT: Production

secrets:
  enabled: true
  connectionString: ""
  jwtSecret: ""
```

---

## Monitoring & Observability

### Application Insights Integration

```csharp
// Program.cs
builder.Services.AddApplicationInsightsTelemetry();
builder.Services.AddApplicationInsightsTelemetryProcessor<HealthCheckFilter>();

// Filter out health check requests
public class HealthCheckFilter : ITelemetryProcessor
{
    private readonly ITelemetryProcessor _next;

    public HealthCheckFilter(ITelemetryProcessor next) => _next = next;

    public void Process(ITelemetry item)
    {
        if (item is RequestTelemetry request &&
            request.Url.AbsolutePath.StartsWith("/health"))
        {
            return; // Skip health checks
        }
        _next.Process(item);
    }
}
```

### Structured Logging with Serilog

```csharp
// Program.cs
Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Information()
    .MinimumLevel.Override("Microsoft", LogEventLevel.Warning)
    .MinimumLevel.Override("Microsoft.Hosting.Lifetime", LogEventLevel.Information)
    .Enrich.FromLogContext()
    .Enrich.WithMachineName()
    .Enrich.WithEnvironmentName()
    .WriteTo.Console(new JsonFormatter())
    .WriteTo.ApplicationInsights(
        TelemetryConfiguration.CreateDefault(),
        TelemetryConverter.Traces)
    .CreateLogger();

builder.Host.UseSerilog();
```

---

## Security Best Practices

### Production Configuration Checklist

| Category | Best Practice |
|----------|---------------|
| **Secrets** | Use Azure Key Vault, never store in code/config |
| **HTTPS** | Force HTTPS, use TLS 1.2+ |
| **Headers** | Add security headers (CSP, HSTS, X-Frame-Options) |
| **CORS** | Configure specific origins, avoid wildcards |
| **Auth** | Use short-lived tokens, implement refresh tokens |
| **Containers** | Run as non-root, use minimal base images |
| **Network** | Use Network Policies, limit egress |
| **Scanning** | Regular dependency and container scanning |

### Security Headers Middleware

```csharp
app.Use(async (context, next) =>
{
    context.Response.Headers.Append("X-Content-Type-Options", "nosniff");
    context.Response.Headers.Append("X-Frame-Options", "DENY");
    context.Response.Headers.Append("X-XSS-Protection", "1; mode=block");
    context.Response.Headers.Append("Referrer-Policy", "strict-origin-when-cross-origin");
    context.Response.Headers.Append(
        "Content-Security-Policy",
        "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'");

    if (context.Request.IsHttps)
    {
        context.Response.Headers.Append(
            "Strict-Transport-Security",
            "max-age=31536000; includeSubDomains");
    }

    await next();
});
```

---

## Quick Reference Commands

```bash
# Docker
docker build -t myapp:1.0 .
docker run -p 5000:80 myapp:1.0
docker-compose up -d

# Kubernetes
kubectl apply -f k8s/
kubectl get pods -n myapp
kubectl logs -f deployment/myapp-api -n myapp
kubectl rollout status deployment/myapp-api -n myapp
kubectl rollout undo deployment/myapp-api -n myapp

# Helm
helm install myapp ./charts/aspnetcore-api
helm upgrade myapp ./charts/aspnetcore-api
helm rollback myapp 1

# Azure CLI
az webapp up --name myapp --resource-group mygroup
az acr build --registry myacr --image myapp:v1 .
az aks get-credentials --resource-group mygroup --name myaks
```

---

## Related Resources

- [ASP.NET Core Deployment](https://docs.microsoft.com/aspnet/core/host-and-deploy)
- [Docker Documentation](https://docs.docker.com/)
- [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks)
- [GitHub Actions](https://docs.github.com/actions)
- [Terraform Azure Provider](https://registry.terraform.io/providers/hashicorp/azurerm)
