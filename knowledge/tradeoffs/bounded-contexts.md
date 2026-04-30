# Bounded Contexts

## When to Use

**Good fit**:
- Large systems with multiple subdomains
- Multiple teams working on same codebase
- Different meanings for same terms in different areas
- Need for independent deployability

**Avoid when**:
- Small applications with single team
- Tightly coupled features that share everything
- Early-stage products where boundaries unclear

---

## Context Mapping Tradeoffs

### Shared Kernel
Small set of shared code between contexts.

**Pros**:
- Avoids duplication of common types
- Ensures consistency for shared concepts

**Cons**:
- Creates coupling between contexts
- Changes require coordination
- Use sparingly

```java
// shared-kernel module
public record Money(BigDecimal amount, Currency currency) {
    public Money add(Money other) { ... }
}
```

### Anti-Corruption Layer
Translate external models to internal models.

**Pros**:
- Isolates from external changes
- Models fit internal needs perfectly

**Cons**:
- Additional translation code
- Possible data loss in translation

```java
class IdentityServiceAdapter implements IdentityService {
    Optional<UserInfo> getUserInfo(UserId id) {
        return identityApi.getUser(id.value())
            .map(this::toUserInfo);  // Translate to our model
    }
}
```

---

## Communication Tradeoffs

### Domain Events (Async)

**Pros**:
- Loose coupling
- Better fault isolation
- Scalable

**Cons**:
- Eventual consistency
- Harder to debug
- Message ordering challenges

### Direct Calls (Sync)

**Pros**:
- Immediate consistency
- Simpler mental model
- Easier debugging

**Cons**:
- Tighter coupling
- Cascading failures
- Latency accumulation

---

## Data Ownership Tradeoffs

### Separate Databases

**Pros**:
- Full autonomy
- Independent scaling
- Clear ownership

**Cons**:
- No joins across contexts
- Data duplication
- Consistency challenges

### Shared Database

**Pros**:
- Easy queries
- Immediate consistency
- Less infrastructure

**Cons**:
- Coupling via schema
- Coordination for changes
- Ownership confusion

---

## Guidelines

1. **One team per context**: Clear ownership
2. **Separate databases**: Each context owns its data
3. **API boundaries**: Communicate via defined contracts
4. **No shared entities**: Copy data, don't share references
5. **Eventual consistency**: Accept async updates between contexts
