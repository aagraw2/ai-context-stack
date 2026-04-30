# Immutability

## Why Immutable
- Thread-safe by default
- No defensive copies needed
- Safe as map keys / set elements
- Easier to reason about

## How to Make Immutable
```java
public final class Money {
    private final BigDecimal amount;
    private final Currency currency;

    public Money(BigDecimal amount, Currency currency) {
        this.amount = amount;
        this.currency = currency;
    }

    // Return new instance, don't mutate
    public Money add(Money other) {
        return new Money(this.amount.add(other.amount), currency);
    }
}
```

## Checklist
- Class is `final`
- All fields `private final`
- No setters
- Return copies of mutable fields
- Deep copy in constructor if needed

## Java Records (Java 14+)
```java
record Point(int x, int y) { }
// Immutable by default
```

## Use For
- Value objects (Money, Email, Address)
- DTOs
- Configuration
- Domain identifiers
