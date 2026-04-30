# Entity vs Value Object

## Entity
Has identity. Mutable. Lifecycle matters.

```java
class User {
    private final UserId id;  // Identity
    private String name;       // Can change
    private Email email;       // Can change

    // Equality by ID
    boolean equals(Object o) {
        return id.equals(((User)o).id);
    }
}
```

**Examples**: User, Order, Vehicle, Booking

## Value Object
No identity. Immutable. Equality by attributes.

```java
record Money(BigDecimal amount, Currency currency) {
    // Equality by all fields (automatic with record)

    Money add(Money other) {
        return new Money(amount.add(other.amount), currency);
    }
}
```

**Examples**: Money, Email, Address, DateRange, Coordinates

## Decision Guide

| Question | Entity | Value Object |
|----------|--------|--------------|
| Has unique ID? | Yes | No |
| Can change over time? | Yes | No (replace) |
| Two with same data equal? | No | Yes |
| Track lifecycle? | Yes | No |

## 
- Entities: Main domain objects (User, Vehicle, Ticket)
- Values: Attributes, measurements, identifiers (Money, Location, Email)
