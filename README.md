# Custom Plugin ASP.NET Core

Professional ASP.NET Core development plugin for Claude Code with 3 agents, 3 skills, and complete development infrastructure.

## 📦 Features

### 3 Specialized Agents

1. **ASP.NET Core Backend Specialist** - REST APIs, Entity Framework, enterprise applications
2. **ASP.NET Core DevOps & Infrastructure** - Docker, Azure, CI/CD, Kubernetes
3. **ASP.NET Core Architecture & Design** - SOLID, design patterns, microservices

### 3 Invokable Skills

1. **ASP.NET Core Fundamentals** - C#, project setup, routing, middleware
2. **ASP.NET Core Advanced Development** - EF Core, authentication, testing, patterns
3. **ASP.NET Core DevOps & Production** - Docker, Azure, CI/CD, monitoring

### 3 Slash Commands

- `/learn` - Personalized learning path (Backend, DevOps, or Architect)
- `/project` - Hands-on project templates (12+ projects)
- `/deploy` - Deployment guidance (Azure App Service, Docker, Kubernetes)

## 🚀 Installation

### Single Line
```bash
plugin add custom-plugin-aspnet-core
```

### From Local Directory
```bash
# Clone and navigate
git clone https://github.com/pluginagentmarketplace/custom-plugin-aspnet-core.git
cd custom-plugin-aspnet-core

# In Claude Code:
# Add from ./custom-plugin-aspnet-core
```

## 📚 Usage

### Start Learning
```bash
/learn
```
Choose from 3 tracks:
- **Backend Developer** (12-14 weeks) - Build REST APIs
- **DevOps Engineer** (10-12 weeks) - Deploy to production
- **Architect** (16-18 weeks) - Design enterprise systems

### Get Project Templates
```bash
/project
```
Access 12+ projects:
- Beginner: Todo API, Product Catalog, Authentication
- Intermediate: E-Commerce, Real-time Chat, Blog Platform
- Advanced: Microservices, Multi-tenant SaaS, Analytics
- DevOps: Containerization, Azure Deployment, Kubernetes

### Deploy Application
```bash
/deploy
```
Deployment options:
- Azure App Service
- Docker + Container Registry
- Kubernetes (AKS)
- GitHub Actions CI/CD

## 🏗️ Plugin Structure

```
custom-plugin-aspnet-core/
├── .claude-plugin/
│   └── plugin.json                    # Plugin manifest
├── agents/                            # 3 agents
│   ├── aspnet-core-backend.md
│   ├── aspnet-core-devops.md
│   └── aspnet-core-architecture.md
├── commands/                          # 3 slash commands
│   ├── learn.md
│   ├── project.md
│   └── deploy.md
├── skills/                            # 3 SKILL.md files
│   ├── aspnet-core-fundamentals/SKILL.md
│   ├── aspnet-core-advanced/SKILL.md
│   └── aspnet-core-devops/SKILL.md
├── hooks/
│   └── hooks.json
└── README.md
```

## 📖 Content Overview

### Backend Agent
- ASP.NET Core framework fundamentals
- C# programming essentials
- RESTful API design
- Entity Framework Core
- Database design
- Authentication & Authorization
- Dependency Injection
- Testing and debugging

### DevOps Agent
- Docker containerization
- Azure cloud services
- CI/CD pipelines (GitHub Actions, Azure Pipelines)
- Kubernetes orchestration
- Infrastructure as Code (Terraform)
- Monitoring & logging
- Security and compliance

### Architecture Agent
- SOLID principles
- Design patterns (Repository, Unit of Work, CQRS, etc.)
- Domain-Driven Design
- Microservices architecture
- Event-driven systems
- Performance optimization
- Scalability patterns

## 💡 Learning Paths

### Path 1: Backend Developer (12-14 weeks)
```
C# Fundamentals → ASP.NET Core Basics → API Development → Database
→ Authentication → Testing → Portfolio Projects
```

### Path 2: DevOps Engineer (10-12 weeks)
```
Docker → Azure Cloud → CI/CD Pipelines → Kubernetes
→ Monitoring → Security → Production Setup
```

### Path 3: Architect (16-18 weeks)
```
SOLID & Patterns → Domain-Driven Design → Microservices
→ Advanced Patterns → Real-world Design
```

## 🎯 Quick Start

1. **Install Plugin**
   ```bash
   plugin add custom-plugin-aspnet-core
   ```

2. **Choose Your Path**
   ```bash
   /learn
   ```

3. **Get Project Template**
   ```bash
   /project
   ```

4. **Build & Deploy**
   ```bash
   /deploy
   ```

## 📊 Statistics

| Component | Count | Details |
|-----------|-------|---------|
| Agents | 3 | Backend, DevOps, Architecture |
| Skills | 3 | Fundamentals, Advanced, DevOps |
| Commands | 3 | Learn, Project, Deploy |
| Projects | 12+ | Beginner to Advanced |
| Learning Weeks | 14-18 | Per complete track |
| Code Examples | 50+ | Real-world implementations |

## 🔑 Key Technologies

- **Language:** C#, .NET 8
- **Framework:** ASP.NET Core
- **Database:** Entity Framework Core, SQL Server
- **Cloud:** Azure (App Service, SQL, Container Registry, AKS)
- **Containers:** Docker, Kubernetes
- **CI/CD:** GitHub Actions, Azure Pipelines
- **Monitoring:** Application Insights, Log Analytics
- **Architecture:** SOLID, Design Patterns, Microservices

## 📝 Project Examples

### Beginner
- Todo API with CRUD operations
- Product catalog with categories
- User authentication with JWT

### Intermediate
- E-commerce API (products, orders, payment)
- Real-time chat with SignalR
- Blog platform with comments

### Advanced
- Microservices system (multiple services, messaging)
- Multi-tenant SaaS application
- Real-time analytics platform

### DevOps
- Containerize ASP.NET Core app (Docker)
- Deploy to Azure App Service
- Kubernetes deployment with health checks

## 🚀 Deployment Support

**Azure App Service**
```bash
dotnet publish -c Release
az webapp deployment source config-zip --src app.zip
```

**Docker**
```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0
COPY ./publish /app
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

**Kubernetes**
```yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

## 📚 Resources

- [ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core)
- [C# Language Reference](https://docs.microsoft.com/dotnet/csharp)
- [Entity Framework Core](https://docs.microsoft.com/ef/core)
- [Azure Documentation](https://docs.microsoft.com/azure)
- [Kubernetes Documentation](https://kubernetes.io/docs)

## 🤝 Support

For issues or questions:
- Check [GitHub Issues](https://github.com/pluginagentmarketplace/custom-plugin-aspnet-core/issues)
- Review command documentation (`/learn`, `/project`, `/deploy`)
- Consult agent expertise for specific domains

## 📄 License

MIT License - See LICENSE file for details

## ✨ Features Highlights

✅ Professional enterprise-grade plugin
✅ 3 specialized agents with deep expertise
✅ Comprehensive learning paths (14-18 weeks)
✅ 12+ hands-on projects with code
✅ Complete deployment guidance
✅ Real-world code examples
✅ Best practices and patterns
✅ Production-ready infrastructure

---

**Start your ASP.NET Core journey:**
```bash
/learn
```

Made with ❤️ for ASP.NET Core developers.
