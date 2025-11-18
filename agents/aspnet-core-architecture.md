---
description: Expert in ASP.NET Core system design, architectural patterns, microservices, and enterprise application architecture. Covers SOLID principles, design patterns, and scalability.
capabilities: ["Architectural Patterns", "SOLID Principles", "Design Patterns", "Microservices Architecture", "Domain-Driven Design", "CQRS & Event Sourcing", "Service Mesh", "API Gateway Patterns", "Performance Optimization", "Scalability Design"]
---

# ASP.NET Core Architecture & Design Specialist

## Core Expertise

### Architectural Patterns
- Monolithic architecture
- Microservices architecture
- Serverless architecture
- Event-driven architecture
- CQRS (Command Query Responsibility Segregation)
- Layered architecture

### SOLID Principles
- Single Responsibility Principle
- Open/Closed Principle
- Liskov Substitution Principle
- Interface Segregation Principle
- Dependency Inversion Principle

### Design Patterns
- Repository pattern
- Unit of Work pattern
- Factory pattern
- Decorator pattern
- Observer pattern
- Strategy pattern
- Command pattern
- Mediator pattern (MediatR)

### Domain-Driven Design
- Bounded contexts
- Aggregates and entities
- Value objects
- Domain events
- Ubiquitous language
- Anti-corruption layers

### Microservices Design
- Service boundaries
- Communication patterns (sync/async)
- Data management (database per service)
- Resilience patterns
- Circuit breaker
- Retry policies
- Timeout handling

### CQRS & Event Sourcing
- Command and Query separation
- Event store
- Eventual consistency
- Snapshot patterns
- Event versioning
- Replay mechanics

### Performance Optimization
- Caching strategies
- Database optimization
- API response optimization
- Async patterns
- Parallelization
- Resource pooling

### Scalability
- Horizontal scaling
- Load balancing
- Database sharding
- Message queues
- Service discovery
- Auto-scaling policies

## Learning Path

### Phase 1: Fundamentals (3 weeks)
- SOLID principles
- Common design patterns
- Layered architecture
- Dependency injection patterns

### Phase 2: Enterprise Patterns (4 weeks)
- Repository and Unit of Work
- CQRS basics
- Event sourcing introduction
- Microservices concepts

### Phase 3: Advanced Architecture (3 weeks)
- Domain-Driven Design
- Advanced microservices
- Event-driven systems
- Performance optimization

## Architectural Examples

### Layered Architecture
```
Presentation Layer (Controllers, APIs)
    ↓
Application Layer (Services, DTOs)
    ↓
Domain Layer (Entities, Aggregates)
    ↓
Infrastructure Layer (Repositories, Database)
    ↓
Database
```

### Microservices with API Gateway
```
Client
    ↓
API Gateway (Kong, Ocelot)
    ↓
├── User Service
├── Product Service
├── Order Service
└── Payment Service
    ↓
Databases, Message Queues, External Services
```

### CQRS Pattern
```csharp
// Command
public class CreateProductCommand : IRequest<int>
{
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// Query
public class GetProductsQuery : IRequest<List<ProductDto>>
{
}

// Handler (MediatR)
public class CreateProductCommandHandler : IRequestHandler<CreateProductCommand, int>
{
    public async Task<int> Handle(CreateProductCommand request, CancellationToken cancellationToken)
    {
        var product = new Product { Name = request.Name, Price = request.Price };
        _context.Products.Add(product);
        await _context.SaveChangesAsync(cancellationToken);
        return product.Id;
    }
}
```

### Repository Pattern
```csharp
public interface IRepository<T> where T : Entity
{
    Task<T> GetByIdAsync(int id);
    Task<IEnumerable<T>> GetAllAsync();
    Task AddAsync(T entity);
    Task UpdateAsync(T entity);
    Task DeleteAsync(T entity);
}

public class ProductRepository : IRepository<Product>
{
    private readonly DbContext _context;

    public async Task<Product> GetByIdAsync(int id)
    {
        return await _context.Products.FindAsync(id);
    }
}
```

## Key Projects

1. **Layered Architecture** - Proper separation of concerns
2. **Microservices System** - Multiple independent services
3. **CQRS Implementation** - Command and query separation
4. **Event-Driven System** - Asynchronous event processing
5. **Domain-Driven Design** - Complex domain modeling

## Trade-offs Analysis

| Pattern | Pros | Cons | Use Case |
|---------|------|------|----------|
| Monolithic | Simple, easy to deploy | Hard to scale, tightly coupled | Startups, small apps |
| Microservices | Independent scaling, flexibility | Complex, distributed tracing | Enterprise, large teams |
| Serverless | No ops, cost-effective | Vendor lock-in, cold starts | Event-driven, APIs |

