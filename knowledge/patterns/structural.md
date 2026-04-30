# Structural Design Patterns

## Adapter Pattern

**Intent**: Convert interface of a class into another interface clients expect. Bridge incompatible interfaces.

**When to Use**:
- Integrating legacy or third-party code
- Interface doesn't match what client needs
- Want to reuse existing class with incompatible interface

**Structure**:
```
interface Target {
    request()
}

class Adaptee {
    specificRequest()
}

class Adapter implements Target {
    adaptee: Adaptee
    request() { adaptee.specificRequest() }
}
```

**Example**:
```java
// Third-party payment library
class StripePayment {
    void executeCharge(double amount, String token) { }
}

// Your interface
interface PaymentGateway {
    void pay(Money amount, PaymentDetails details);
}

// Adapter
class StripeAdapter implements PaymentGateway {
    private final StripePayment stripe;

    public void pay(Money amount, PaymentDetails details) {
        stripe.executeCharge(amount.toDouble(), details.getToken());
    }
}
```

---

## Decorator Pattern

**Intent**: Attach additional responsibilities dynamically. Flexible alternative to subclassing.

**When to Use**:
- Add behavior without modifying existing code
- Combine behaviors flexibly at runtime
- Avoid class explosion from feature combinations

**Structure**:
```
interface Component {
    operation()
}

class ConcreteComponent implements Component

class Decorator implements Component {
    wrapped: Component
    operation() { wrapped.operation() + extra }
}
```

**Example**:
```java
interface DataSource {
    void writeData(String data);
    String readData();
}

class EncryptionDecorator implements DataSource {
    private final DataSource wrapped;

    public void writeData(String data) {
        wrapped.writeData(encrypt(data));
    }
}

class CompressionDecorator implements DataSource {
    private final DataSource wrapped;

    public void writeData(String data) {
        wrapped.writeData(compress(data));
    }
}

// Usage: stack decorators
DataSource source = new CompressionDecorator(
    new EncryptionDecorator(
        new FileDataSource("file.txt")
    )
);
```

---

## Facade Pattern

**Intent**: Provide simplified interface to complex subsystem.

**When to Use**:
- Subsystem is complex with many classes
- Want to decouple clients from subsystem
- Need simple entry point to functionality

**Example**:
```java
class OrderFacade {
    private final InventoryService inventory;
    private final PaymentService payment;
    private final ShippingService shipping;
    private final NotificationService notifications;

    public OrderResult placeOrder(Order order) {
        inventory.reserve(order.getItems());
        payment.charge(order.getTotal());
        shipping.schedule(order.getAddress());
        notifications.sendConfirmation(order.getEmail());
        return OrderResult.success();
    }
}
```

---

## Repository Pattern

**Intent**: Abstract data access behind collection-like interface. Mediates between domain and data mapping.

**When to Use**:
- Decouple domain from persistence
- Enable testability with in-memory implementations
- Centralize query logic

**Structure**:
```
interface Repository<T, ID> {
    find(id: ID): T
    findAll(): List<T>
    save(entity: T): T
    delete(id: ID): void
}
```

**Example**:
```java
public interface UserRepository {
    Optional<User> findById(UserId id);
    Optional<User> findByEmail(Email email);
    List<User> findByRole(Role role);
    User save(User user);
    void delete(UserId id);
}

// Implementation in infrastructure layer
class JpaUserRepository implements UserRepository {
    // JPA implementation
}

// For testing
class InMemoryUserRepository implements UserRepository {
    private final Map<UserId, User> store = new HashMap<>();
}
```

---

## Composite Pattern

**Intent**: Compose objects into tree structures. Treat individual objects and compositions uniformly.

**When to Use**:
- Represent part-whole hierarchies
- Clients should treat composites and individuals uniformly
- Tree-like object structure

**Example**:
```java
interface Component {
    int getPrice();
}

class Product implements Component {
    private final int price;
    public int getPrice() { return price; }
}

class Box implements Component {
    private final List<Component> contents;
    public int getPrice() {
        return contents.stream().mapToInt(Component::getPrice).sum();
    }
}
```
