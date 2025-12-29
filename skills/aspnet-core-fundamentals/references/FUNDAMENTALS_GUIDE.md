# ASP.NET Core Fundamentals Guide

## Overview

This guide covers essential ASP.NET Core concepts for building modern web applications and APIs.

## Table of Contents

1. [Project Setup](#project-setup)
2. [Controllers and Routing](#controllers-and-routing)
3. [Dependency Injection](#dependency-injection)
4. [Middleware Pipeline](#middleware-pipeline)
5. [Model Binding and Validation](#model-binding-and-validation)
6. [Configuration](#configuration)
7. [Best Practices](#best-practices)

---

## Project Setup

### Creating a New Project

```bash
# Create Web API project
dotnet new webapi -n MyApp

# Create MVC project
dotnet new mvc -n MyMvcApp

# Create Minimal API
dotnet new web -n MyMinimalApp
```

### Project File (.csproj)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>
</Project>
```

---

## Controllers and Routing

### Basic Controller

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly IProductService _productService;

    public ProductsController(IProductService productService)
    {
        _productService = productService;
    }

    [HttpGet]
    public async Task<ActionResult<IEnumerable<ProductDto>>> GetAll()
    {
        var products = await _productService.GetAllAsync();
        return Ok(products);
    }

    [HttpGet("{id}")]
    public async Task<ActionResult<ProductDto>> GetById(int id)
    {
        var product = await _productService.GetByIdAsync(id);
        if (product == null)
            return NotFound();
        return Ok(product);
    }

    [HttpPost]
    public async Task<ActionResult<ProductDto>> Create(CreateProductDto dto)
    {
        var product = await _productService.CreateAsync(dto);
        return CreatedAtAction(nameof(GetById), new { id = product.Id }, product);
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> Update(int id, UpdateProductDto dto)
    {
        await _productService.UpdateAsync(id, dto);
        return NoContent();
    }

    [HttpDelete("{id}")]
    public async Task<IActionResult> Delete(int id)
    {
        await _productService.DeleteAsync(id);
        return NoContent();
    }
}
```

### Route Constraints

```csharp
[HttpGet("{id:int:min(1)}")]       // Integer, minimum 1
[HttpGet("{name:alpha}")]          // Alphabetic characters only
[HttpGet("{date:datetime}")]       // DateTime format
[HttpGet("{guid:guid}")]           // GUID format
[HttpGet("{filename:regex(^.+\\.pdf$)}")] // Regex pattern
```

---

## Dependency Injection

### Service Registration

```csharp
public static class DependencyInjection
{
    public static IServiceCollection AddApplicationServices(
        this IServiceCollection services)
    {
        // Singleton - One instance for app lifetime
        services.AddSingleton<ICacheService, MemoryCacheService>();

        // Scoped - One instance per request
        services.AddScoped<IProductService, ProductService>();
        services.AddScoped<IUnitOfWork, UnitOfWork>();

        // Transient - New instance every time
        services.AddTransient<IValidator<Product>, ProductValidator>();

        return services;
    }
}
```

### Constructor Injection

```csharp
public class ProductService : IProductService
{
    private readonly IRepository<Product> _repository;
    private readonly IMapper _mapper;
    private readonly ILogger<ProductService> _logger;

    public ProductService(
        IRepository<Product> repository,
        IMapper mapper,
        ILogger<ProductService> logger)
    {
        _repository = repository;
        _mapper = mapper;
        _logger = logger;
    }
}
```

---

## Middleware Pipeline

### Custom Middleware

```csharp
public class RequestLoggingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<RequestLoggingMiddleware> _logger;

    public RequestLoggingMiddleware(
        RequestDelegate next,
        ILogger<RequestLoggingMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var stopwatch = Stopwatch.StartNew();

        _logger.LogInformation(
            "Request {Method} {Path} started",
            context.Request.Method,
            context.Request.Path);

        await _next(context);

        stopwatch.Stop();
        _logger.LogInformation(
            "Request {Method} {Path} completed in {ElapsedMs}ms",
            context.Request.Method,
            context.Request.Path,
            stopwatch.ElapsedMilliseconds);
    }
}

// Extension method for registration
public static class MiddlewareExtensions
{
    public static IApplicationBuilder UseRequestLogging(
        this IApplicationBuilder builder)
    {
        return builder.UseMiddleware<RequestLoggingMiddleware>();
    }
}
```

### Pipeline Configuration

```csharp
var app = builder.Build();

// Order matters!
app.UseExceptionHandler("/error");
app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseRouting();
app.UseCors("AllowSpecificOrigins");
app.UseAuthentication();
app.UseAuthorization();
app.UseRequestLogging();
app.MapControllers();
```

---

## Model Binding and Validation

### Model with Data Annotations

```csharp
public class CreateProductDto
{
    [Required(ErrorMessage = "Name is required")]
    [StringLength(100, MinimumLength = 3)]
    public string Name { get; set; } = null!;

    [StringLength(500)]
    public string? Description { get; set; }

    [Required]
    [Range(0.01, 10000.00)]
    public decimal Price { get; set; }

    [Range(0, int.MaxValue)]
    public int StockQuantity { get; set; }

    [Required]
    public int CategoryId { get; set; }
}
```

### Binding Sources

```csharp
[HttpPost]
public IActionResult Create(
    [FromBody] CreateProductDto dto,      // Request body (JSON)
    [FromQuery] int? page,                 // Query string
    [FromRoute] int id,                    // Route parameter
    [FromHeader] string authorization,     // Request header
    [FromServices] IProductService service) // DI container
{
    // Implementation
}
```

---

## Configuration

### appsettings.json

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=MyApp;Trusted_Connection=True;"
  },
  "AppSettings": {
    "PageSize": 10,
    "MaxUploadSize": 5242880
  }
}
```

### Strongly-Typed Configuration

```csharp
public class AppSettings
{
    public int PageSize { get; set; }
    public long MaxUploadSize { get; set; }
}

// Registration
services.Configure<AppSettings>(
    configuration.GetSection("AppSettings"));

// Usage
public class MyService
{
    private readonly AppSettings _settings;

    public MyService(IOptions<AppSettings> options)
    {
        _settings = options.Value;
    }
}
```

---

## Best Practices

### 1. Use Async/Await Consistently

```csharp
// Good
public async Task<Product> GetProductAsync(int id)
{
    return await _context.Products.FindAsync(id);
}

// Avoid
public Task<Product> GetProductAsync(int id)
{
    return _context.Products.FindAsync(id).AsTask();
}
```

### 2. Return Appropriate Status Codes

| Status Code | Use Case |
|-------------|----------|
| 200 OK | Successful GET, PUT |
| 201 Created | Successful POST |
| 204 No Content | Successful DELETE, PUT with no body |
| 400 Bad Request | Validation errors |
| 401 Unauthorized | Missing/invalid authentication |
| 403 Forbidden | Authenticated but not authorized |
| 404 Not Found | Resource doesn't exist |
| 500 Internal Error | Unhandled exceptions |

### 3. Use DTOs for API Contracts

```csharp
// Don't expose entities directly
public async Task<ActionResult<Product>> GetProduct(int id) // Bad

// Use DTOs
public async Task<ActionResult<ProductDto>> GetProduct(int id) // Good
```

### 4. Implement Global Exception Handling

```csharp
app.UseExceptionHandler(errorApp =>
{
    errorApp.Run(async context =>
    {
        context.Response.StatusCode = 500;
        context.Response.ContentType = "application/json";

        var error = context.Features.Get<IExceptionHandlerFeature>();
        if (error != null)
        {
            await context.Response.WriteAsJsonAsync(new
            {
                Message = "An error occurred",
                TraceId = Activity.Current?.Id ?? context.TraceIdentifier
            });
        }
    });
});
```

---

## Quick Reference

### Common NuGet Packages

| Package | Purpose |
|---------|---------|
| Microsoft.AspNetCore.OpenApi | Swagger/OpenAPI |
| AutoMapper.Extensions.Microsoft.DependencyInjection | Object mapping |
| FluentValidation.AspNetCore | Validation |
| Serilog.AspNetCore | Structured logging |
| Polly | Resilience policies |

### CLI Commands

```bash
dotnet new webapi -n MyApi        # Create project
dotnet add package <name>          # Add package
dotnet build                       # Build project
dotnet run                         # Run application
dotnet watch run                   # Run with hot reload
dotnet test                        # Run tests
dotnet publish -c Release          # Publish for deployment
```

---

## Related Resources

- [Microsoft ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core)
- [ASP.NET Core GitHub Repository](https://github.com/dotnet/aspnetcore)
- [.NET API Browser](https://docs.microsoft.com/dotnet/api)
