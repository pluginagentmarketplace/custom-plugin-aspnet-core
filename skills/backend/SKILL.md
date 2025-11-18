---
name: backend-api-skills
description: Master Backend & API Development skills including server architecture, databases, authentication, microservices, and REST/GraphQL APIs.
---

# Backend & API Development Skills

## Core Foundation

### Step 1: Server Basics (2-3 weeks)
- HTTP protocol and REST principles
- Request/response cycles
- Routing and middleware
- Error handling

### Step 2: Database Mastery (3-4 weeks)
- SQL fundamentals (SELECT, INSERT, UPDATE, DELETE)
- Database design and normalization
- Indexes and optimization
- NoSQL basics (MongoDB, Redis)

### Step 3: Authentication & Security (2-3 weeks)
- Password hashing (bcrypt, argon2)
- JWT tokens
- OAuth 2.0
- HTTPS and encryption

### Step 4: Advanced Patterns (4-6 weeks)
- Microservices architecture
- Message queues (RabbitMQ, Kafka)
- Caching strategies
- API rate limiting

## Technology Comparison

### Node.js vs Python vs Java vs Go
| Aspect | Node.js | Python | Java | Go |
|--------|---------|--------|------|-----|
| Speed | Fast | Slower | Very Fast | Very Fast |
| Learning | Easy | Easy | Hard | Medium |
| Async | Native | asyncio | Threads | Goroutines |
| Best For | APIs, Real-time | Scripting, ML | Enterprise | Microservices |

## Backend Stack Setup

```
Framework: Express / FastAPI / Spring Boot / Gin
Database: PostgreSQL / MySQL / MongoDB
ORM: Sequelize / SQLAlchemy / Hibernate / GORM
Auth: JWT / Passport.js / Spring Security
Testing: Jest / Pytest / JUnit / Go test
Deployment: Docker + Kubernetes / Cloud Run
```

## API Development

### REST API Design
```
GET /api/users          - Get all users
GET /api/users/:id      - Get single user
POST /api/users         - Create user
PUT /api/users/:id      - Update user
DELETE /api/users/:id   - Delete user
```

### GraphQL Alternative
```graphql
query {
  users(limit: 10) {
    id
    name
    email
  }
}
```

## Database Optimization

### Indexing Strategy
- Create index on frequently queried columns
- Composite indexes for common filters
- Monitor slow queries

### Connection Pooling
- Reuse database connections
- Set appropriate pool size
- Handle connection timeouts

### Caching Layer
- Redis for session data
- Cache frequent queries
- Implement cache invalidation

## Testing Strategy

```
Unit Tests (70%) - Individual functions
  ↓
Integration Tests (20%) - Database + API
  ↓
E2E Tests (10%) - Full user flows
```

## Key Competencies

1. **HTTP Protocol** - Methods, status codes, headers
2. **Database Design** - Schemas, relationships
3. **API Design** - RESTful principles, versioning
4. **Authentication** - Secure credential handling
5. **Performance** - Optimization, caching
6. **Security** - Input validation, SQL injection prevention
7. **Testing** - Coverage and best practices
8. **Deployment** - Cloud platforms, CI/CD
9. **Monitoring** - Logging, error tracking
10. **Documentation** - OpenAPI/Swagger

## Real-World Project: E-commerce API

### Endpoints
- Products: GET, POST, PUT, DELETE
- Shopping Cart: CRUD operations
- Orders: Placement, tracking
- Payments: Integration with Stripe
- Users: Authentication, profile management

### Database Schema
- Users table
- Products table
- Orders table
- Order items table
- Payments table

### Security Measures
- JWT authentication
- Password hashing
- Input validation
- Rate limiting
- HTTPS only

