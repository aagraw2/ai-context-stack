# Layered Architecture

## When to Use

**Good fit**:
- Enterprise applications with complex business logic
- Long-lived systems requiring maintainability
- Teams needing clear separation of concerns
- Systems requiring testability in isolation

**Avoid when**:
- Simple CRUD applications (over-engineering)
- Prototypes or throwaway code
- Very small microservices

---

## Dependency Rule

**Dependencies point inward (toward domain)**

```
Presentation -> Application -> Domain <- Infrastructure
```

- Domain has ZERO external dependencies
- Infrastructure implements domain interfaces
- Application orchestrates domain objects
- Presentation handles HTTP/UI concerns

---

## Layer Responsibilities

### Presentation Layer
**Purpose**: Handle external communication (HTTP, CLI, WebSocket)

**Contains**:
- Controllers / Handlers
- Request/Response DTOs
- Input validation
- Authentication/Authorization checks
- Error response formatting

**Tradeoff**: Thin vs thick controllers
- Thin: Controllers only translate HTTP <-> commands, all logic in application layer
- Thick: Some coordination logic in controllers for simpler flows

---

### Application Layer
**Purpose**: Orchestrate use cases, coordinate domain objects

**Contains**:
- Use cases / Application services
- Command/Query objects
- Transaction boundaries
- Event publishing

**Rules**:
- No business logic (delegate to domain)
- No infrastructure concerns
- Thin orchestration only

**Tradeoff**: CQRS vs unified services
- CQRS: Separate read/write models for complex queries
- Unified: Simpler when read/write requirements are similar

---

### Domain Layer
**Purpose**: Core business logic and rules

**Contains**:
- Entities (with identity)
- Value Objects (immutable, no identity)
- Domain Services (stateless logic across entities)
- Domain Events
- Repository Interfaces (not implementations)

**Rules**:
- NO framework dependencies
- NO infrastructure dependencies
- Pure business logic

**Tradeoff**: Rich vs anemic domain model
- Rich: Entities contain behavior, more OOP
- Anemic: Entities are data holders, logic in services

---

### Infrastructure Layer
**Purpose**: Technical implementations, external systems

**Contains**:
- Repository implementations
- Database access (JPA, JDBC)
- External API clients
- Message queue adapters
- Caching implementations

**Tradeoff**: Framework coupling
- Heavy: Use framework annotations throughout (faster development)
- Light: Isolate framework code, more portable

---

## Anti-Patterns

### Leaky Abstractions
```java
// Bad: Domain knows about JPA
@Entity
class User {
    @Id private Long id;
}

// Better: Domain is pure
class User {
    private UserId id;
}

// JPA entity in infrastructure
@Entity
class UserEntity {
    @Id private Long id;
}
```

### Circular dependencies
Application -> Domain -> Application (broken!)

### Skipping layers
Presentation -> Domain directly (bypassing application layer)
