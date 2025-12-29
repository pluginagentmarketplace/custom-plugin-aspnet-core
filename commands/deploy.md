---
name: deploy
description: ASP.NET Core Deployment Guide
allowed-tools: Read
---

# /deploy - ASP.NET Core Deployment Guide

**Deploy your ASP.NET Core application to production.**

## Deployment Options

### Option 1: Azure App Service (Recommended)

#### Step 1: Create Azure Resources
```bash
# Create Resource Group
az group create --name myResourceGroup --location eastus

# Create App Service Plan
az appservice plan create \
  --name myAppServicePlan \
  --resource-group myResourceGroup \
  --sku B1 \
  --is-linux

# Create Web App
az webapp create \
  --resource-group myResourceGroup \
  --plan myAppServicePlan \
  --name myapp-<unique-suffix> \
  --runtime "DOTNET:8.0"
```

#### Step 2: Prepare Application
```bash
# Publish application
dotnet publish -c Release -o ./publish

# Create deployment package
cd publish
zip -r ../app.zip .
cd ..
```

#### Step 3: Deploy
```bash
# Deploy to App Service
az webapp deployment source config-zip \
  --resource-group myResourceGroup \
  --name myapp-<unique-suffix> \
  --src app.zip

# Verify deployment
az webapp show \
  --resource-group myResourceGroup \
  --name myapp-<unique-suffix>
```

#### Step 4: Configure Database
```bash
# Create Azure SQL Database
az sql server create \
  --resource-group myResourceGroup \
  --name myserver \
  --admin-user sqladmin \
  --admin-password P@ssw0rd

# Create database
az sql db create \
  --resource-group myResourceGroup \
  --server myserver \
  --name myappdb

# Get connection string
az sql db show-connection-string \
  --client ado.net \
  --server myserver \
  --name myappdb
```

#### Step 5: Setup Configuration
```bash
# Add connection string to App Service
az webapp config appsettings set \
  --resource-group myResourceGroup \
  --name myapp-<unique-suffix> \
  --settings ConnectionStrings__DefaultConnection="<connection-string>"

# Setup application settings
az webapp config appsettings set \
  --resource-group myResourceGroup \
  --name myapp-<unique-suffix> \
  --settings ASPNETCORE_ENVIRONMENT=Production
```

### Option 2: Docker + Container Registry

#### Step 1: Build Docker Image
```bash
docker build -t myapp:1.0 .
docker tag myapp:1.0 myregistry.azurecr.io/myapp:1.0
```

#### Step 2: Push to Azure Container Registry
```bash
# Create registry
az acr create \
  --resource-group myResourceGroup \
  --name myregistry \
  --sku Basic

# Login
az acr login --name myregistry

# Push image
docker push myregistry.azurecr.io/myapp:1.0
```

#### Step 3: Create Container Instance
```bash
az container create \
  --resource-group myResourceGroup \
  --name myapp-container \
  --image myregistry.azurecr.io/myapp:1.0 \
  --registry-login-server myregistry.azurecr.io \
  --registry-username <username> \
  --registry-password <password> \
  --environment-variables ASPNETCORE_ENVIRONMENT=Production \
  --ports 80 443 \
  --protocol TCP
```

### Option 3: Kubernetes (AKS)

#### Step 1: Create AKS Cluster
```bash
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 3 \
  --vm-set-type VirtualMachineScaleSets \
  --network-plugin azure \
  --service-cidr 10.0.0.0/16 \
  --dns-service-ip 10.0.0.10
```

#### Step 2: Get Credentials
```bash
az aks get-credentials \
  --resource-group myResourceGroup \
  --name myAKSCluster
```

#### Step 3: Create Kubernetes Manifests
```yaml
# deployment.yaml
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
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 500m
            memory: 512Mi
        livenessProbe:
          httpGet:
            path: /health
            port: 80
          initialDelaySeconds: 10
          periodSeconds: 10

---
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: LoadBalancer
```

#### Step 4: Deploy
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

# Get load balancer IP
kubectl get service myapp-service
```

## CI/CD Pipeline (GitHub Actions)

```yaml
name: Deploy to Azure

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - name: Setup .NET
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '8.0'

    - name: Publish
      run: dotnet publish -c Release -o ./publish

    - name: Upload Artifact
      uses: actions/upload-artifact@v2
      with:
        name: webapp
        path: ./publish

    - name: Deploy to Azure App Service
      uses: azure/webapps-deploy@v2
      with:
        app-name: myapp-<unique-suffix>
        publish-profile: ${{ secrets.AZURE_PUBLISH_PROFILE }}
        package: ./publish
```

## Post-Deployment

### Health Checks
```bash
# Check application status
curl https://myapp-<unique-suffix>.azurewebsites.net/health

# Expected response: 200 OK
```

### Monitoring Setup
```bash
# Create Application Insights
az monitor app-insights component create \
  --app myapp-insights \
  --location eastus \
  --resource-group myResourceGroup

# Link to App Service
az webapp config set \
  --resource-group myResourceGroup \
  --name myapp-<unique-suffix> \
  --app-insights-key <instrumentation-key>
```

### Database Migrations
```bash
# Run migrations after deployment
dotnet ef database update --configuration Release
```

## Troubleshooting

```bash
# View logs
az webapp log tail \
  --resource-group myResourceGroup \
  --name myapp-<unique-suffix>

# Check resource status
az webapp show \
  --resource-group myResourceGroup \
  --name myapp-<unique-suffix> \
  --query state

# Restart application
az webapp restart \
  --resource-group myResourceGroup \
  --name myapp-<unique-suffix>
```

## Production Checklist

- [ ] Database backups configured
- [ ] SSL/TLS certificate installed
- [ ] Monitoring and alerts setup
- [ ] Application Insights enabled
- [ ] Logging configured
- [ ] Health checks implemented
- [ ] Auto-scaling configured
- [ ] Disaster recovery plan ready
- [ ] Security scans completed
- [ ] Performance tested

