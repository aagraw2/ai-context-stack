# Dependency Injection

## When to Use

**Good fit**:
- Systems requiring testability
- Swappable implementations (DB, APIs)
- Complex object graphs
- Long-lived applications

**Avoid when**:
- Simple scripts or utilities
- Performance-critical paths (DI has overhead)
- Very small applications

---

## Injection Style Tradeoffs

### Constructor Injection (Preferred)

**Pros**:
- Dependencies explicit
- Immutable fields
- Object fully initialized
- Easy to test

**Cons**:
- Many constructor parameters visible

```java
class UserService {
    private final UserRepository userRepository;
    private final EmailService emailService;

    UserService(UserRepository userRepository, EmailService emailService) {
        this.userRepository = userRepository;
        this.emailService = emailService;
    }
}
```

### Field Injection

**Pros**:
- Less boilerplate
- Quick to add dependencies

**Cons**:
- Hidden dependencies
- Harder to test
- Requires reflection/framework

```java
// Avoid in production code
class OrderService {
    @Inject
    private OrderRepository repository;  // Hidden
}
```

### Setter Injection

**Pros**:
- Optional dependencies
- Reconfigurable at runtime

**Cons**:
- Object partially initialized
- Mutable state

---

## Wiring Tradeoffs

### Manual Wiring

**Pros**:
- Explicit, type-safe
- No magic
- Compile-time errors

**Cons**:
- Verbose
- Manual updates when graph changes

### Auto-wiring (Framework)

**Pros**:
- Less boilerplate
- Automatic resolution

**Cons**:
- Runtime errors
- Harder to follow
- Framework coupling

---

## Anti-Patterns

### Service Locator
```java
// Bad - hidden dependency
class OrderService {
    void process(Order order) {
        var repository = ServiceLocator.get(OrderRepository.class);
        repository.save(order);
    }
}
```

**Problems**:
- Dependencies not visible in API
- Hard to test
- Global state

### Too Many Dependencies
```java
// Class doing too much
class GodService {
    GodService(A a, B b, C c, D d, E e, F f, G g) { }
}
```

**Fix**: Break into smaller, focused classes

---

## Interface Segregation for DI

Split large interfaces so clients only depend on what they need:

```java
// Instead of one large interface
interface UserRepository {
    User findById(UserId id);
    List<User> findAll();
    User save(User user);
    void delete(UserId id);
}

// Split by use case
interface UserReader {
    Optional<User> findById(UserId id);
}

interface UserWriter {
    User save(User user);
    void delete(UserId id);
}

// Inject only what's needed
class UserQueryService {
    private final UserReader users;  // Only needs read
}
```
