---
name: project
description: ASP.NET Core Project Templates
allowed-tools: Read
---

# /project - ASP.NET Core Project Templates

**Get hands-on project templates and implementation guidance.**

## Beginner Projects

### Project 1: Simple Todo API (2-3 weeks)
**Skills:** Controllers, Entity Framework, CRUD operations

```bash
dotnet new webapi -n TodoApi
cd TodoApi
```

**Requirements:**
- Create Todo model with Id, Title, IsCompleted
- GET /api/todos - Get all todos
- GET /api/todos/{id} - Get single todo
- POST /api/todos - Create todo
- PUT /api/todos/{id} - Update todo
- DELETE /api/todos/{id} - Delete todo

**Learning Goals:**
- ASP.NET Core routing
- Model binding
- Entity Framework basic CRUD
- HTTP methods
- Status codes

### Project 2: Product Catalog API (3-4 weeks)
**Skills:** Relationships, Validation, Filtering

**Features:**
- Products and Categories
- One-to-many relationship
- Filtering and pagination
- Data validation
- Error handling

### Project 3: User Authentication (3-4 weeks)
**Skills:** Identity, JWT, Authorization

**Features:**
- User registration
- Login with JWT tokens
- Protected endpoints
- Role-based access
- Password hashing

## Intermediate Projects

### Project 4: E-Commerce API (6-8 weeks)
**Skills:** Complex relationships, transactions, business logic

**Features:**
- Products, Categories, Orders
- Shopping cart
- Order management
- Inventory tracking
- Payment integration

**Database Schema:**
```
Users
├─ Orders
│  └─ OrderItems
│     └─ Products
└─ Carts
   └─ CartItems
```

### Project 5: Real-time Chat API (4-5 weeks)
**Skills:** SignalR, Real-time communication

**Features:**
- User connections
- Group messaging
- Online status
- Message history
- Typing indicators

### Project 6: Blog Platform API (5-6 weeks)
**Skills:** Complex domain model, comments, notifications

**Features:**
- Posts and comments
- User profiles
- Tags and categories
- Notifications
- Search functionality

## Advanced Projects

### Project 7: Microservices System (8-10 weeks)
**Skills:** Service independence, messaging, resilience

**Services:**
- User Service
- Product Service
- Order Service
- Payment Service
- Notification Service

**Communication:**
- API Gateway
- RabbitMQ messaging
- gRPC for internal services
- Event-driven architecture

### Project 8: Multi-tenant SaaS (8-10 weeks)
**Skills:** Tenant isolation, data security

**Features:**
- Tenant management
- Isolated databases
- Row-level security
- Billing system
- Feature flags per tenant

### Project 9: Real-time Analytics (6-8 weeks)
**Skills:** SignalR, data aggregation, caching

**Features:**
- Event collection
- Real-time dashboards
- Data aggregation
- Report generation
- Export functionality

## Docker & Deployment Projects

### Project 10: Containerization (2-3 weeks)
**Task:** Containerize any existing ASP.NET Core app

```bash
docker build -t myapp .
docker run -p 8080:80 myapp
```

### Project 11: Azure Deployment (3-4 weeks)
**Task:** Deploy application to Azure

```bash
az appservice plan create --name myplan --resource-group mygroup --sku B1
az webapp create --resource-group mygroup --plan myplan --name myapp
az webapp deployment source config-zip --resource-group mygroup --name myapp --src app.zip
```

### Project 12: Kubernetes Setup (4-5 weeks)
**Task:** Deploy to Kubernetes cluster

```yaml
# Create deployment, service, and ingress
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

## GitHub Repository Setup

```bash
# Initialize new project
git init
git add .
git commit -m "Initial ASP.NET Core project"
git branch -M main
git remote add origin https://github.com/yourname/yourproject.git
git push -u origin main
```

## Project Checklist Template

For each project, ensure:

- [ ] Unit tests (80%+ coverage)
- [ ] Integration tests
- [ ] API documentation (Swagger)
- [ ] Error handling
- [ ] Logging setup
- [ ] Validation
- [ ] Security best practices
- [ ] Performance optimization
- [ ] Docker containerization
- [ ] GitHub repository with README

## Next Steps

Run `/deploy` for deployment guidance once project is ready.

