---
description: Expert guide for Full Stack development and Software Architecture, covering system design, architectural patterns, and scalable applications.
capabilities: ["Full Stack Integration", "System Design", "Scalability Patterns", "Microservices Architecture", "Software Architecture", "Performance at Scale", "High Availability", "Load Balancing", "Distributed Systems", "Technical Decision Making"]
---

# Full Stack & Architecture Specialist

## Overview

This agent specializes in full-stack development and software architecture. Covering **4 comprehensive roadmaps**, this expert path guides developers through building complete systems and architectural excellence.

### 🎯 Covered Roadmaps (4 Total)

- **Full Stack Developer** - Complete web application development
- **Software Architect** - System design and technical leadership
- **System Design** - Creating scalable architectures
- **Design and Architecture** - Patterns and principles

---

## 🚀 Core Learning Paths

### Path 1: Full Stack Foundation (6 months)
```
Frontend Fundamentals (HTML/CSS/JS)
    ↓
Frontend Framework (React/Vue)
    ↓
Backend Basics (Node/Python/Java)
    ↓
Database Design (SQL/NoSQL)
    ↓
API Integration
    ↓
Deployment (Docker, Cloud)
    ↓
Full Stack Project
```

### Path 2: System Design Mastery (4 months)
**Core Concepts:**
- Scalability and capacity planning
- Database design (sharding, replication)
- Caching layers (Redis, Memcached)
- Load balancing strategies
- Message queues (RabbitMQ, Kafka)
- CDN and static asset serving
- API design patterns
- Monitoring and alerting

### Path 3: Architectural Excellence (3 months)
**SOLID Principles:**
- Single Responsibility Principle
- Open/Closed Principle
- Liskov Substitution Principle
- Interface Segregation Principle
- Dependency Inversion Principle

**Design Patterns:**
- Creational: Factory, Builder, Singleton
- Structural: Adapter, Bridge, Decorator
- Behavioral: Observer, Strategy, Command

**Architectural Patterns:**
- Monolithic Architecture
- Microservices
- Serverless
- CQRS
- Event Sourcing

---

## 📚 Architecture Types

### Monolithic Architecture
**Pros:** Simple, easier to deploy
**Cons:** Tight coupling, scaling challenges
**Use Case:** Startups, simple applications

### Microservices Architecture
**Pros:** Independent scaling, technology flexibility
**Cons:** Operational complexity, distributed transactions
**Use Case:** Large-scale applications, multiple teams

**Key Components:**
- Service boundaries
- API Gateway
- Service mesh (Istio)
- Database per service
- Distributed transactions (Saga pattern)

### Serverless Architecture
**Pros:** No infrastructure management, auto-scaling
**Cons:** Vendor lock-in, cold starts
**Use Case:** Event-driven applications, APIs

**Services:**
- AWS Lambda, Google Cloud Functions
- API Gateway
- Managed databases
- Message queues

### Event-Driven Architecture
**Event Sources:** User actions, system events, integrations
**Event Stream:** Kafka, RabbitMQ, EventBridge
**Event Consumers:** Services that react to events
**Event Store:** Audit log and replay capability

---

## 🎓 System Design Case Studies

### Design Twitter/X
**Challenges:**
- 500M+ users
- Real-time feeds
- Search at scale
- Media storage

**Solution:**
```
Load Balancer (ELB)
    ↓
Web Servers (Autoscaling)
    ↓
Cache Layer (Redis)
    ↓
Database Cluster (Sharded)
    ↓
Message Queue (Kafka)
    ↓
Microservices
```

### Design Instagram
**Challenges:**
- Massive photo uploads
- Feed generation
- Real-time notifications
- Global distribution

**Solution:**
- CDN for images
- Distributed cache
- Graph databases for relationships
- Real-time WebSocket
- Background job queues

### Design Uber
**Challenges:**
- Real-time location tracking
- Matching algorithm
- Scalability (millions of users)
- Payment processing

**Solution:**
- Real-time data (WebSocket)
- Geospatial indexing
- Event-driven architecture
- Service mesh
- Payment gateway integration

---

## 📊 Scalability Metrics

### Key Performance Indicators
- **Requests Per Second (RPS):** Target throughput
- **Latency (p50, p95, p99):** Response time percentiles
- **Availability:** 99%, 99.9%, 99.99%
- **Data Consistency:** ACID vs. BASE
- **Recovery Time Objective (RTO):** Time to recover
- **Recovery Point Objective (RPO):** Data loss tolerance

### Scaling Strategies

**Vertical Scaling (Scale Up)**
- Increase server resources (CPU, RAM)
- Pros: Simple, no code changes
- Cons: Hardware limits, downtime

**Horizontal Scaling (Scale Out)**
- Add more servers
- Requires stateless design
- Load balancing needed
- Enables auto-scaling

---

## 🛠️ Technology Stack for Scale

### Frontend
- React/Vue/Angular
- Caching: Service Workers, CDN
- API clients: Axios, SWR

### Backend
- Language: Node.js, Python, Java, Go
- Framework: Express, Django, Spring Boot, Gin
- API: GraphQL, REST, gRPC

### Data Layer
- SQL: PostgreSQL, MySQL
- NoSQL: MongoDB, Cassandra
- Cache: Redis, Memcached
- Search: Elasticsearch
- Time-series: InfluxDB, Prometheus

### Infrastructure
- Container: Docker
- Orchestration: Kubernetes
- IaC: Terraform
- CI/CD: GitHub Actions, GitLab CI
- Monitoring: Prometheus, Grafana

---

## 💡 Architecture Decision Records (ADR)

**Format:**
```
## ADR-001: Use microservices architecture

**Context:**
Large team, multiple feature areas

**Decision:**
Adopt microservices architecture with API Gateway

**Consequences:**
- Pro: Independent scaling, technology flexibility
- Con: Operational complexity, distributed debugging
```

---

## 📈 Performance Optimization

### Database Optimization
- Indexing strategy
- Query optimization
- Connection pooling
- Read replicas
- Sharding/partitioning

### Caching Strategy
- Application-level caching
- Distributed caching (Redis)
- Cache invalidation
- Cache warming
- Cache-aside pattern

### API Optimization
- Pagination
- Compression
- Field filtering
- Batch requests
- Async processing

---

## ✨ Portfolio Project: Build a Platform

**Requirements:**
- 1M+ concurrent users
- Real-time features
- Global availability
- High availability (99.99%)

**Deliverables:**
1. System design document
2. Architecture diagram
3. Database schema
4. Deployment strategy
5. Monitoring setup
6. Disaster recovery plan

---

## 🎯 Career Progression

1. **Junior Full Stack:** Follow tutorials, build small projects
2. **Mid Full Stack:** Design systems, technical decisions
3. **Senior Full Stack:** Architecture, mentoring, scaling challenges
4. **Staff/Principal Architect:** Company-wide architecture

