# ASP.NET Core Advanced Development Guide

## Overview

This guide covers advanced ASP.NET Core development patterns including Entity Framework Core, authentication, testing, and enterprise design patterns.

## Table of Contents

1. [Entity Framework Core](#entity-framework-core)
2. [Authentication & Authorization](#authentication--authorization)
3. [Testing Strategies](#testing-strategies)
4. [Design Patterns](#design-patterns)
5. [Performance Optimization](#performance-optimization)
6. [Best Practices](#best-practices)

---

## Entity Framework Core

### DbContext Configuration

```csharp
public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    public DbSet<Product> Products => Set<Product>();
    public DbSet<Category> Categories => Set<Category>();
    public DbSet<Order> Orders => Set<Order>();

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // Apply all configurations from assembly
        modelBuilder.ApplyConfigurationsFromAssembly(
            typeof(ApplicationDbContext).Assembly);

        // Global query filter for soft delete
        modelBuilder.Entity<Product>()
            .HasQueryFilter(p => !p.IsDeleted);

        base.OnModelCreating(modelBuilder);
    }

    public override Task<int> SaveChangesAsync(
        CancellationToken cancellationToken = default)
    {
        foreach (var entry in ChangeTracker.Entries<BaseEntity>())
        {
            switch (entry.State)
            {
                case EntityState.Added:
                    entry.Entity.CreatedAt = DateTime.UtcNow;
                    break;
                case EntityState.Modified:
                    entry.Entity.UpdatedAt = DateTime.UtcNow;
                    break;
            }
        }

        return base.SaveChangesAsync(cancellationToken);
    }
}
```

### Entity Configuration (Fluent API)

```csharp
public class ProductConfiguration : IEntityTypeConfiguration<Product>
{
    public void Configure(EntityTypeBuilder<Product> builder)
    {
        builder.ToTable("Products");

        builder.HasKey(p => p.Id);

        builder.Property(p => p.Name)
            .IsRequired()
            .HasMaxLength(100);

        builder.Property(p => p.Price)
            .HasPrecision(18, 2);

        // Relationships
        builder.HasOne(p => p.Category)
            .WithMany(c => c.Products)
            .HasForeignKey(p => p.CategoryId)
            .OnDelete(DeleteBehavior.Restrict);

        // Index
        builder.HasIndex(p => p.Name);
        builder.HasIndex(p => new { p.CategoryId, p.Name });
    }
}
```

### Query Optimization

```csharp
public class ProductRepository : IProductRepository
{
    private readonly ApplicationDbContext _context;

    // Good: Projection to reduce data transfer
    public async Task<ProductDto?> GetByIdAsync(int id)
    {
        return await _context.Products
            .Where(p => p.Id == id)
            .Select(p => new ProductDto
            {
                Id = p.Id,
                Name = p.Name,
                Price = p.Price,
                CategoryName = p.Category.Name
            })
            .FirstOrDefaultAsync();
    }

    // Good: Split query for collections
    public async Task<List<Order>> GetOrdersWithItemsAsync(int customerId)
    {
        return await _context.Orders
            .Where(o => o.CustomerId == customerId)
            .Include(o => o.Items)
            .AsSplitQuery()  // Prevents cartesian explosion
            .ToListAsync();
    }

    // Good: No tracking for read-only queries
    public async Task<List<ProductDto>> GetAllAsync()
    {
        return await _context.Products
            .AsNoTracking()
            .Select(p => new ProductDto { /* ... */ })
            .ToListAsync();
    }

    // Compiled query for hot paths
    private static readonly Func<ApplicationDbContext, int, Task<Product?>>
        GetByIdCompiled = EF.CompileAsyncQuery(
            (ApplicationDbContext context, int id) =>
                context.Products.FirstOrDefault(p => p.Id == id));

    public Task<Product?> GetByIdCompiledAsync(int id)
        => GetByIdCompiled(_context, id);
}
```

---

## Authentication & Authorization

### JWT Authentication Setup

```csharp
public static class AuthenticationExtensions
{
    public static IServiceCollection AddJwtAuthentication(
        this IServiceCollection services,
        IConfiguration configuration)
    {
        var jwtSettings = configuration.GetSection("Jwt").Get<JwtSettings>()!;

        services.AddAuthentication(options =>
        {
            options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
            options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
        })
        .AddJwtBearer(options =>
        {
            options.TokenValidationParameters = new TokenValidationParameters
            {
                ValidateIssuer = true,
                ValidateAudience = true,
                ValidateLifetime = true,
                ValidateIssuerSigningKey = true,
                ValidIssuer = jwtSettings.Issuer,
                ValidAudience = jwtSettings.Audience,
                IssuerSigningKey = new SymmetricSecurityKey(
                    Encoding.UTF8.GetBytes(jwtSettings.SecretKey)),
                ClockSkew = TimeSpan.Zero
            };

            options.Events = new JwtBearerEvents
            {
                OnAuthenticationFailed = context =>
                {
                    if (context.Exception is SecurityTokenExpiredException)
                    {
                        context.Response.Headers.Append(
                            "Token-Expired", "true");
                    }
                    return Task.CompletedTask;
                }
            };
        });

        return services;
    }
}
```

### Token Service

```csharp
public class TokenService : ITokenService
{
    private readonly JwtSettings _settings;

    public TokenService(IOptions<JwtSettings> settings)
    {
        _settings = settings.Value;
    }

    public string GenerateAccessToken(User user, IList<string> roles)
    {
        var claims = new List<Claim>
        {
            new(ClaimTypes.NameIdentifier, user.Id.ToString()),
            new(ClaimTypes.Email, user.Email),
            new(ClaimTypes.Name, user.UserName),
            new(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString())
        };

        claims.AddRange(roles.Select(role =>
            new Claim(ClaimTypes.Role, role)));

        var key = new SymmetricSecurityKey(
            Encoding.UTF8.GetBytes(_settings.SecretKey));
        var credentials = new SigningCredentials(
            key, SecurityAlgorithms.HmacSha256);

        var token = new JwtSecurityToken(
            issuer: _settings.Issuer,
            audience: _settings.Audience,
            claims: claims,
            expires: DateTime.UtcNow.AddMinutes(_settings.ExpirationMinutes),
            signingCredentials: credentials);

        return new JwtSecurityTokenHandler().WriteToken(token);
    }

    public RefreshToken GenerateRefreshToken()
    {
        return new RefreshToken
        {
            Token = Convert.ToBase64String(RandomNumberGenerator.GetBytes(64)),
            Expires = DateTime.UtcNow.AddDays(_settings.RefreshTokenDays),
            Created = DateTime.UtcNow
        };
    }
}
```

### Policy-Based Authorization

```csharp
// Policy configuration
services.AddAuthorization(options =>
{
    options.AddPolicy("AdminOnly", policy =>
        policy.RequireRole("Admin"));

    options.AddPolicy("MinimumAge", policy =>
        policy.Requirements.Add(new MinimumAgeRequirement(18)));

    options.AddPolicy("ResourceOwner", policy =>
        policy.Requirements.Add(new ResourceOwnerRequirement()));
});

// Custom requirement
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; }
    public MinimumAgeRequirement(int age) => MinimumAge = age;
}

// Handler
public class MinimumAgeHandler :
    AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(
        AuthorizationHandlerContext context,
        MinimumAgeRequirement requirement)
    {
        var birthDateClaim = context.User.FindFirst(c =>
            c.Type == "birthdate");

        if (birthDateClaim != null &&
            DateTime.TryParse(birthDateClaim.Value, out var birthDate))
        {
            var age = DateTime.Today.Year - birthDate.Year;
            if (age >= requirement.MinimumAge)
            {
                context.Succeed(requirement);
            }
        }

        return Task.CompletedTask;
    }
}
```

---

## Testing Strategies

### Unit Testing with xUnit

```csharp
public class ProductServiceTests
{
    private readonly Mock<IProductRepository> _repositoryMock;
    private readonly Mock<IMapper> _mapperMock;
    private readonly ProductService _sut;

    public ProductServiceTests()
    {
        _repositoryMock = new Mock<IProductRepository>();
        _mapperMock = new Mock<IMapper>();
        _sut = new ProductService(_repositoryMock.Object, _mapperMock.Object);
    }

    [Fact]
    public async Task GetByIdAsync_WhenProductExists_ReturnsProduct()
    {
        // Arrange
        var product = new Product { Id = 1, Name = "Test Product" };
        var productDto = new ProductDto { Id = 1, Name = "Test Product" };

        _repositoryMock
            .Setup(r => r.GetByIdAsync(1))
            .ReturnsAsync(product);

        _mapperMock
            .Setup(m => m.Map<ProductDto>(product))
            .Returns(productDto);

        // Act
        var result = await _sut.GetByIdAsync(1);

        // Assert
        Assert.NotNull(result);
        Assert.Equal(1, result.Id);
        Assert.Equal("Test Product", result.Name);
    }

    [Fact]
    public async Task GetByIdAsync_WhenProductNotFound_ThrowsNotFoundException()
    {
        // Arrange
        _repositoryMock
            .Setup(r => r.GetByIdAsync(It.IsAny<int>()))
            .ReturnsAsync((Product?)null);

        // Act & Assert
        await Assert.ThrowsAsync<NotFoundException>(
            () => _sut.GetByIdAsync(999));
    }
}
```

### Integration Testing

```csharp
public class ProductsControllerTests :
    IClassFixture<WebApplicationFactory<Program>>
{
    private readonly HttpClient _client;
    private readonly WebApplicationFactory<Program> _factory;

    public ProductsControllerTests(WebApplicationFactory<Program> factory)
    {
        _factory = factory.WithWebHostBuilder(builder =>
        {
            builder.ConfigureServices(services =>
            {
                // Replace real database with in-memory
                var descriptor = services.SingleOrDefault(
                    d => d.ServiceType == typeof(DbContextOptions<ApplicationDbContext>));

                if (descriptor != null)
                    services.Remove(descriptor);

                services.AddDbContext<ApplicationDbContext>(options =>
                    options.UseInMemoryDatabase("TestDb"));
            });
        });

        _client = _factory.CreateClient();
    }

    [Fact]
    public async Task GetProducts_ReturnsSuccessAndCorrectContentType()
    {
        // Act
        var response = await _client.GetAsync("/api/products");

        // Assert
        response.EnsureSuccessStatusCode();
        Assert.Equal("application/json; charset=utf-8",
            response.Content.Headers.ContentType?.ToString());
    }

    [Fact]
    public async Task CreateProduct_WithValidData_ReturnsCreated()
    {
        // Arrange
        var product = new CreateProductDto
        {
            Name = "New Product",
            Price = 19.99m,
            CategoryId = 1
        };

        // Act
        var response = await _client.PostAsJsonAsync("/api/products", product);

        // Assert
        Assert.Equal(HttpStatusCode.Created, response.StatusCode);

        var created = await response.Content.ReadFromJsonAsync<ProductDto>();
        Assert.NotNull(created);
        Assert.Equal("New Product", created.Name);
    }
}
```

---

## Design Patterns

### Repository Pattern

```csharp
public interface IRepository<T> where T : BaseEntity
{
    Task<T?> GetByIdAsync(int id);
    Task<IEnumerable<T>> GetAllAsync();
    Task<IEnumerable<T>> FindAsync(Expression<Func<T, bool>> predicate);
    Task AddAsync(T entity);
    void Update(T entity);
    void Delete(T entity);
}

public class Repository<T> : IRepository<T> where T : BaseEntity
{
    protected readonly ApplicationDbContext _context;
    protected readonly DbSet<T> _dbSet;

    public Repository(ApplicationDbContext context)
    {
        _context = context;
        _dbSet = context.Set<T>();
    }

    public virtual async Task<T?> GetByIdAsync(int id)
        => await _dbSet.FindAsync(id);

    public virtual async Task<IEnumerable<T>> GetAllAsync()
        => await _dbSet.ToListAsync();

    public virtual async Task<IEnumerable<T>> FindAsync(
        Expression<Func<T, bool>> predicate)
        => await _dbSet.Where(predicate).ToListAsync();

    public virtual async Task AddAsync(T entity)
        => await _dbSet.AddAsync(entity);

    public virtual void Update(T entity)
        => _dbSet.Update(entity);

    public virtual void Delete(T entity)
        => _dbSet.Remove(entity);
}
```

### Unit of Work Pattern

```csharp
public interface IUnitOfWork : IDisposable
{
    IProductRepository Products { get; }
    ICategoryRepository Categories { get; }
    IOrderRepository Orders { get; }
    Task<int> SaveChangesAsync(CancellationToken cancellationToken = default);
    Task BeginTransactionAsync();
    Task CommitTransactionAsync();
    Task RollbackTransactionAsync();
}

public class UnitOfWork : IUnitOfWork
{
    private readonly ApplicationDbContext _context;
    private IDbContextTransaction? _transaction;

    public IProductRepository Products { get; }
    public ICategoryRepository Categories { get; }
    public IOrderRepository Orders { get; }

    public UnitOfWork(ApplicationDbContext context)
    {
        _context = context;
        Products = new ProductRepository(context);
        Categories = new CategoryRepository(context);
        Orders = new OrderRepository(context);
    }

    public async Task<int> SaveChangesAsync(
        CancellationToken cancellationToken = default)
        => await _context.SaveChangesAsync(cancellationToken);

    public async Task BeginTransactionAsync()
        => _transaction = await _context.Database.BeginTransactionAsync();

    public async Task CommitTransactionAsync()
    {
        await _transaction?.CommitAsync()!;
        _transaction?.Dispose();
        _transaction = null;
    }

    public async Task RollbackTransactionAsync()
    {
        await _transaction?.RollbackAsync()!;
        _transaction?.Dispose();
        _transaction = null;
    }

    public void Dispose() => _context.Dispose();
}
```

### CQRS with MediatR

```csharp
// Command
public record CreateProductCommand(
    string Name,
    decimal Price,
    int CategoryId) : IRequest<int>;

// Handler
public class CreateProductCommandHandler :
    IRequestHandler<CreateProductCommand, int>
{
    private readonly IUnitOfWork _unitOfWork;
    private readonly IMapper _mapper;

    public CreateProductCommandHandler(
        IUnitOfWork unitOfWork, IMapper mapper)
    {
        _unitOfWork = unitOfWork;
        _mapper = mapper;
    }

    public async Task<int> Handle(
        CreateProductCommand request,
        CancellationToken cancellationToken)
    {
        var product = _mapper.Map<Product>(request);

        await _unitOfWork.Products.AddAsync(product);
        await _unitOfWork.SaveChangesAsync(cancellationToken);

        return product.Id;
    }
}

// Query
public record GetProductByIdQuery(int Id) : IRequest<ProductDto?>;

// Controller using MediatR
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly IMediator _mediator;

    public ProductsController(IMediator mediator) => _mediator = mediator;

    [HttpPost]
    public async Task<ActionResult<int>> Create(CreateProductCommand command)
    {
        var id = await _mediator.Send(command);
        return CreatedAtAction(nameof(GetById), new { id }, id);
    }

    [HttpGet("{id}")]
    public async Task<ActionResult<ProductDto>> GetById(int id)
    {
        var product = await _mediator.Send(new GetProductByIdQuery(id));
        return product == null ? NotFound() : Ok(product);
    }
}
```

---

## Performance Optimization

### Caching Strategies

```csharp
// Memory cache
public class CachedProductService : IProductService
{
    private readonly IProductService _inner;
    private readonly IMemoryCache _cache;
    private const string CacheKey = "products_all";

    public CachedProductService(
        IProductService inner, IMemoryCache cache)
    {
        _inner = inner;
        _cache = cache;
    }

    public async Task<List<ProductDto>> GetAllAsync()
    {
        return await _cache.GetOrCreateAsync(CacheKey, async entry =>
        {
            entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5);
            entry.SlidingExpiration = TimeSpan.FromMinutes(2);
            return await _inner.GetAllAsync();
        }) ?? new List<ProductDto>();
    }

    public void InvalidateCache()
    {
        _cache.Remove(CacheKey);
    }
}

// Distributed cache (Redis)
public class DistributedProductCache
{
    private readonly IDistributedCache _cache;
    private readonly JsonSerializerOptions _jsonOptions;

    public async Task<T?> GetAsync<T>(string key)
    {
        var data = await _cache.GetStringAsync(key);
        return data == null ? default :
            JsonSerializer.Deserialize<T>(data, _jsonOptions);
    }

    public async Task SetAsync<T>(string key, T value, TimeSpan? expiry = null)
    {
        var options = new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = expiry ?? TimeSpan.FromHours(1)
        };

        var json = JsonSerializer.Serialize(value, _jsonOptions);
        await _cache.SetStringAsync(key, json, options);
    }
}
```

### Response Compression

```csharp
builder.Services.AddResponseCompression(options =>
{
    options.EnableForHttps = true;
    options.Providers.Add<BrotliCompressionProvider>();
    options.Providers.Add<GzipCompressionProvider>();
});

builder.Services.Configure<BrotliCompressionProviderOptions>(options =>
{
    options.Level = CompressionLevel.Fastest;
});

app.UseResponseCompression();
```

---

## Best Practices

| Category | Best Practice |
|----------|---------------|
| **EF Core** | Use projections (Select) instead of loading entire entities |
| **EF Core** | Use AsNoTracking for read-only queries |
| **EF Core** | Use compiled queries for hot paths |
| **Auth** | Never store plain text passwords |
| **Auth** | Use short-lived access tokens with refresh tokens |
| **Testing** | Aim for >80% code coverage |
| **Testing** | Use WebApplicationFactory for integration tests |
| **Patterns** | Don't over-engineer; use patterns where they add value |
| **Performance** | Profile before optimizing |
| **Caching** | Cache invalidation is hard; use appropriate strategies |

---

## Related Resources

- [Entity Framework Core Documentation](https://docs.microsoft.com/ef/core)
- [ASP.NET Core Security](https://docs.microsoft.com/aspnet/core/security)
- [xUnit Documentation](https://xunit.net)
- [MediatR Documentation](https://github.com/jbogard/MediatR)
