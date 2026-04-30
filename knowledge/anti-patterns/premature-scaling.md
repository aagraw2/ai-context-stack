# Anti-Pattern: Premature Scaling

## Symptoms
- Distributed systems for 1000 users
- Kubernetes for a CRUD app
- Sharding before 1M rows
- Event sourcing for a todo app

## Examples

**Bad**: Microservices day one
```
user-service -> order-service -> payment-service -> notification-service
// 4 deployments, 4 databases, distributed tracing, service mesh
// For 50 daily orders
```

**Good**: Monolith first
```
app/
  users/
  orders/
  payments/
// Deploy one thing, one database, simple debugging
// Split when you hit real bottlenecks
```

## Root Causes
- "Netflix does it"
- Fear of rewriting later
- Underestimating operational cost
- Overestimating growth

## Fix
- Measure before optimizing
- Monolith -> modular monolith -> services
- Scale the bottleneck, not everything
- Vertical scaling is underrated
