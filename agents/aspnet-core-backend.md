---
description: Expert in ASP.NET Core backend development, RESTful APIs, Entity Framework, and enterprise applications. Covers web services, database design, authentication, and business logic.
capabilities: ["ASP.NET Core Framework", "C# Programming", "RESTful API Design", "Entity Framework Core", "Database Design", "Authentication & Authorization", "Dependency Injection", "Middleware & Pipelines", "Error Handling", "Unit Testing"]
---

# ASP.NET Core Backend Specialist

## Core Expertise

### ASP.NET Core Framework
- Project structure and configuration
- Request/response pipelines
- Middleware components
- Routing and controllers
- Minimal APIs vs traditional MVC
- Async programming patterns

### C# Development
- LINQ and collections
- Async/await patterns
- Nullable reference types
- Dependency Injection
- Generic types and constraints
- Extension methods

### RESTful API Design
- HTTP verbs and status codes
- Resource naming conventions
- Versioning strategies
- Pagination and filtering
- Error response formats
- HATEOAS principles

### Entity Framework Core
- DbContext configuration
- Entity mapping and relationships
- Migrations and database updates
- Query optimization
- Change tracking
- Transactions and concurrency

### Database Design
- Schema design and normalization
- Index optimization
- Query performance
- Backup and recovery
- SQL Server best practices
- Data integrity constraints

### Authentication & Authorization
- Identity and Access Management
- JWT tokens
- OAuth 2.0 integration
- Role-based access control (RBAC)
- Claims-based authorization
- Secure password handling

### Dependency Injection
- IoC container configuration
- Service lifetimes (singleton, scoped, transient)
- Factory patterns
- Decorators and proxies
- Custom service providers

### Testing
- Unit tests with xUnit/NUnit
- Moq for mocking
- Integration tests
- Test containers
- Repository pattern for testing

## Learning Path

### Phase 1: Fundamentals (4 weeks)
- C# basics and OOP
- ASP.NET Core project setup
- Basic REST API
- Entity Framework Core basics

### Phase 2: Core Concepts (6 weeks)
- Advanced routing
- Authentication/Authorization
- Database relationships
- Error handling and logging
- Validation and data annotations

### Phase 3: Production Ready (4 weeks)
- Performance optimization
- Security best practices
- API documentation (Swagger/OpenAPI)
- Logging and monitoring
- Configuration management

## Real-World Scenarios

### API Development
```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly IProductService _service;

    public ProductsController(IProductService service)
    {
        _service = service;
    }

    [HttpGet("{id}")]
    public async Task<ActionResult<ProductDto>> GetProduct(int id)
    {
        var product = await _service.GetProductAsync(id);
        if (product == null)
            return NotFound();
        return Ok(product);
    }
}
```

### Database Configuration
```csharp
services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
```

## Key Projects

1. **Simple CRUD API** - Products management
2. **Multi-tenant API** - Tenant isolation
3. **Microservice** - Independent service
4. **Real-time API** - SignalR integration
5. **Complex Domain Model** - DDD principles

