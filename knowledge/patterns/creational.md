# Creational Design Patterns

## Factory Pattern

**Intent**: Create objects without specifying exact class. Delegate instantiation to subclasses or factory methods.

**When to Use**:
- Object creation logic is complex
- Need to decouple client from concrete classes
- Want to centralize object creation

**Structure**:
```
interface Product
class ConcreteProductA implements Product
class ConcreteProductB implements Product

class Factory {
    create(type): Product
}
```

**Example**:
```java
public interface NotificationSender {
    void send(String message);
}

public class NotificationFactory {
    public static NotificationSender create(String channel) {
        return switch (channel) {
            case "email" -> new EmailSender();
            case "sms" -> new SmsSender();
            case "push" -> new PushSender();
            default -> throw new IllegalArgumentException("Unknown channel");
        };
    }
}
```

---

## Builder Pattern

**Intent**: Construct complex objects step-by-step. Separate construction from representation.

**When to Use**:
- Object has many optional parameters
- Construction requires multiple steps
- Want immutable objects with clean API

**Structure**:
```
class Product {
    static Builder builder()
}

class Builder {
    withX(x): Builder
    withY(y): Builder
    build(): Product
}
```

**Example**:
```java
User user = User.builder()
    .name("John")
    .email("john@example.com")
    .role(Role.ADMIN)
    .build();
```

---

## Singleton Pattern

**Intent**: Ensure class has only one instance with global access point.

**When to Use**:
- Exactly one instance needed (config, connection pool)
- Global access required
- Lazy initialization beneficial

**Caution**: Often overused. Prefer dependency injection.

**Example**:
```java
public class Configuration {
    private static volatile Configuration instance;

    public static Configuration getInstance() {
        if (instance == null) {
            synchronized (Configuration.class) {
                if (instance == null) {
                    instance = new Configuration();
                }
            }
        }
        return instance;
    }
}
```

---

## Prototype Pattern

**Intent**: Create objects by cloning existing instance.

**When to Use**:
- Object creation is expensive
- Need copies with slight variations
- Runtime object composition

**Example**:
```java
public interface Prototype<T> {
    T clone();
}

public class Document implements Prototype<Document> {
    public Document clone() {
        return new Document(this.title, this.content, this.metadata);
    }
}
```
