# Builder vs Factory

## Factory
Creates object in one call. Hides concrete type.

```java
interface Notification { }

class NotificationFactory {
    static Notification create(String type) {
        return switch(type) {
            case "email" -> new EmailNotification();
            case "sms" -> new SmsNotification();
            default -> throw new IllegalArgumentException();
        };
    }
}
```

**Use when**: Creation logic, hide implementation, return interface

## Builder
Step-by-step construction. Many optional params.

```java
User user = User.builder()
    .name("John")
    .email("john@example.com")
    .age(25)           // optional
    .address(addr)     // optional
    .build();
```

**Use when**: Many params, optional fields, immutable objects

## Decision Guide

| Situation | Use |
|-----------|-----|
| Hide concrete class | Factory |
| 4+ constructor params | Builder |
| Optional params | Builder |
| Return different types | Factory |
| Immutable with many fields | Builder |
| Simple creation, few params | Constructor |
