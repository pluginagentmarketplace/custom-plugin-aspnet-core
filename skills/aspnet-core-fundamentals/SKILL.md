---
name: aspnet-core-fundamentals
description: Master ASP.NET Core fundamentals including C#, project structure, routing, middleware, and basic API development. Essential skills for all ASP.NET Core developers.
sasmp_version: "1.3.0"
bonded_agent: aspnet-core-backend
bond_type: PRIMARY_BOND
---

# ASP.NET Core Fundamentals

## Core Skills

### C# Essentials
- Variables, types, operators
- Control flow (if, switch, loops)
- Functions and methods
- Classes and inheritance
- Interfaces and abstractions
- Properties and auto-properties
- LINQ queries
- Async/await patterns

### ASP.NET Core Project Setup
- Project creation (dotnet new)
- Configuration (appsettings.json)
- Dependency Injection setup
- Startup configuration
- Environment-specific settings
- NuGet package management

### Routing & Controllers
- Attribute routing
- Route constraints
- Action results
- Model binding
- Parameter binding
- Conventional routing
- Route templates

### Middleware Pipeline
- Request/response cycle
- Custom middleware
- Error handling middleware
- Logging middleware
- CORS configuration
- Static files serving
- Authentication/Authorization middleware

### Models & Data Binding
- Model classes
- Data annotations
- Model validation
- Custom validators
- Binding sources (route, query, body)
- Complex type binding

### Views & Razor (if using MVC)
- Razor syntax
- Tag helpers
- HTML helpers
- Partial views
- Layout pages
- View components

## Learning Progression

### Week 1-2: C# Basics
- Data types and variables
- Control structures
- Methods and functions
- Classes and OOP

### Week 3-4: ASP.NET Core Setup
- Project structure
- Dependency Injection
- Configuration
- Startup pipeline

### Week 5-6: APIs & Routing
- Creating controllers
- REST principles
- Routing configuration
- Action results

### Week 7-8: Data & Validation
- Model binding
- Validation
- Entity models
- Database context basics

## Code Examples

### Simple API Endpoint
```csharp
[ApiController]
[Route("api/[controller]")]
public class HelloController : ControllerBase
{
    [HttpGet]
    public IActionResult Get()
    {
        return Ok(new { message = "Hello ASP.NET Core" });
    }
}
```

### Middleware Configuration
```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    app.UseAuthorization();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

### Dependency Injection
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<IProductService, ProductService>();
    services.AddControllers();
}
```

## Assessment Criteria

- [ ] Can create a new ASP.NET Core project
- [ ] Understand request/response pipeline
- [ ] Write basic REST APIs
- [ ] Use dependency injection
- [ ] Apply validation to models
- [ ] Configure middleware
- [ ] Handle errors properly
- [ ] Use async/await correctly

