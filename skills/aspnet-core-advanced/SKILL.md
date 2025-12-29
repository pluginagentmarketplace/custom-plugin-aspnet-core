---
name: aspnet-core-advanced
description: Master advanced ASP.NET Core development including Entity Framework Core, authentication, testing, and enterprise patterns for production applications.
sasmp_version: "1.3.0"
bonded_agent: aspnet-core-architecture
bond_type: PRIMARY_BOND
---

# ASP.NET Core Advanced Development

## Advanced Skills

### Entity Framework Core
- DbContext configuration
- Fluent API mapping
- Relationships (one-to-many, many-to-many)
- Migrations and database updates
- Query optimization
- LINQ performance
- Transactions
- Change tracking
- Interceptors and events

### Authentication & Authorization
- Identity framework
- JWT tokens
- Cookie authentication
- Claims-based authorization
- Role-based access control
- Policy-based authorization
- Custom authentication handlers
- External providers (OAuth2, OpenID Connect)

### Advanced API Design
- Content negotiation
- Custom formatters
- API versioning
- OpenAPI/Swagger documentation
- Custom model binders
- Custom action filters
- Problem details responses

### Testing
- Unit testing (xUnit, NUnit)
- Mocking (Moq, NSubstitute)
- Integration testing
- WebApplicationFactory
- Test containers
- Fixture-based testing
- Repository pattern for testing

### Performance & Optimization
- Query analysis
- N+1 problem solving
- Lazy loading vs eager loading
- Select N + 1 prevention
- Caching strategies
- Database indexing
- Connection pooling
- Asynchronous operations

### Logging & Monitoring
- Structured logging
- Serilog integration
- Application Insights
- Performance counters
- Custom metrics
- Health checks
- Diagnostics

### Advanced Patterns
- Repository pattern
- Unit of Work pattern
- Service layer pattern
- CQRS pattern
- Mediator pattern (MediatR)
- Factory patterns
- Decorator pattern

## Advanced Examples

### Entity Framework with Relationships
```csharp
public class Order : Entity
{
    public int CustomerId { get; set; }
    public Customer Customer { get; set; }
    public List<OrderItem> Items { get; set; }
}

public class OrderItem : Entity
{
    public int OrderId { get; set; }
    public int ProductId { get; set; }
    public Product Product { get; set; }
    public decimal Price { get; set; }
    public int Quantity { get; set; }
}

// Fluent API configuration
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .HasMany(o => o.Items)
        .WithOne(i => i.Order)
        .HasForeignKey(i => i.OrderId);
}
```

### JWT Authentication
```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            IssuerSigningKey = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes(Configuration["Jwt:Key"]))
        };
    });
```

### Testing with WebApplicationFactory
```csharp
public class ApiTests : IClassFixture<WebApplicationFactory<Program>>
{
    private readonly WebApplicationFactory<Program> _factory;

    public async Task GetProduct_ReturnsOk()
    {
        var client = _factory.CreateClient();
        var response = await client.GetAsync("/api/products/1");

        Assert.Equal(HttpStatusCode.OK, response.StatusCode);
    }
}
```

## Assessment Criteria

- [ ] Design and implement database schemas
- [ ] Implement authentication and authorization
- [ ] Write comprehensive unit tests
- [ ] Optimize database queries
- [ ] Implement caching strategies
- [ ] Create proper API documentation
- [ ] Handle errors gracefully
- [ ] Implement monitoring and logging
- [ ] Use design patterns appropriately
- [ ] Optimize application performance

