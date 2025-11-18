---
name: fullstack-architecture-skills
description: Master Full Stack development and Software Architecture including system design, scalability patterns, and complete application development.
---

# Full Stack & Architecture Skills

## Bridging Frontend and Backend

### Full Stack Competencies
1. Frontend + Backend integration
2. Database design and optimization
3. API development and consumption
4. Deployment and DevOps
5. Monitoring and maintenance
6. Security across layers

## System Design Principles

### Scalability Dimensions
- **Vertical Scaling:** More power per server
- **Horizontal Scaling:** More servers
- **Database Scaling:** Sharding, replication
- **Caching:** Reduce database load

### Common Patterns
- **Monolithic:** Simple, tightly coupled
- **Microservices:** Complex, independent
- **Serverless:** Event-driven, managed
- **Hybrid:** Best of both worlds

## Architecture Decision Matrix

| Component | Monolith | Microservices | Serverless |
|-----------|----------|---------------|-----------|
| Complexity | Low | High | Medium |
| Scalability | Hard | Easy | Auto |
| Cost | Predictable | Variable | Pay-per-use |
| Team Size | Small | Large | Medium |

## End-to-End Architecture

```
Client (Browser/Mobile)
    ↓ HTTPS
Load Balancer
    ↓
API Gateway
    ↓
Microservices (Backend)
    ↓
Database Layer (Primary/Replica)
    ↓
Cache Layer (Redis)
    ↓
Message Queue (Kafka)
    ↓
External Services (Payment, Email)
```

## Performance Optimization

### Frontend
- Code splitting and lazy loading
- Image optimization
- CDN for static assets
- Caching strategies

### Backend
- Database indexing
- Query optimization
- Connection pooling
- Async processing

### Infrastructure
- Load balancing
- Auto-scaling
- Content delivery
- Monitoring

## Key Skills

1. **System Design** - Architecture decision making
2. **Database Design** - Schema optimization
3. **API Design** - RESTful/GraphQL
4. **Frontend State** - State management
5. **Backend Patterns** - Microservices, CQRS
6. **DevOps** - Deployment, infrastructure
7. **Monitoring** - Performance, errors
8. **Security** - Authentication, encryption

