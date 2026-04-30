# Case Study: Bounded Contexts Implementation

## Context Map

"User" means different things in different contexts:

```
+------------------+      +------------------+
|    Identity      |      |     Billing      |
|    Context       |----->|     Context      |
|                  |      |                  |
|  - User          |      |  - Customer      |
|  - Credentials   |      |  - Invoice       |
|  - Session       |      |  - Subscription  |
+------------------+      +------------------+
         |
         |
         v
+------------------+
|    Support       |
|    Context       |
|                  |
|  - Agent         |
|  - Ticket        |
|  - Customer      |
+------------------+
```

---

## Module Structure

Each bounded context becomes a module:

```
modules/
+-- identity/
|   +-- domain/
|   +-- application/
|   +-- infrastructure/
|   +-- api/
+-- billing/
|   +-- domain/
|   +-- application/
|   +-- infrastructure/
|   +-- api/
+-- support/
    +-- domain/
    +-- application/
    +-- infrastructure/
    +-- api/
```

---

## Inter-Context Communication

### Domain Events (Async)

```java
// Identity context publishes
class UserRegisteredEvent {
    UserId userId;
    String email;
    Instant registeredAt;
}

// Billing context subscribes
class CreateCustomerOnUserRegistration {
    @EventHandler
    void handle(UserRegisteredEvent event) {
        Customer customer = Customer.create(event.userId(), event.email());
        customerRepository.save(customer);
    }
}
```

### Anti-Corruption Layer (Sync)

```java
// Billing context needs identity data
// Don't depend directly on Identity module

interface IdentityService {
    Optional<UserInfo> getUserInfo(UserId id);
}

// Implementation in infrastructure
class IdentityServiceAdapter implements IdentityService {
    private final IdentityModuleApi identityApi;

    Optional<UserInfo> getUserInfo(UserId id) {
        // Translate external model to our context's model
        return identityApi.getUser(id.value())
            .map(this::toUserInfo);
    }
}
```

---

## Shared Kernel Example

```java
// shared-kernel module - use sparingly
public record Money(BigDecimal amount, Currency currency) {
    public Money add(Money other) { ... }
    public Money multiply(double factor) { ... }
}
```

## Published Language Example

```java
// published in identity-api module
public record UserRegisteredEvent {
    String userId;
    String email;
    String timestamp;
}
```
