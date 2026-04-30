# Enums

## Basic Enum
```java
enum OrderStatus {
    PENDING, CONFIRMED, SHIPPED, DELIVERED, CANCELLED
}
```

## Enum with Data
```java
enum PaymentMethod {
    CREDIT_CARD(2.5),
    DEBIT_CARD(1.0),
    UPI(0.0);

    private final double feePercent;
    PaymentMethod(double fee) { this.feePercent = fee; }
    public double getFee() { return feePercent; }
}
```

## Enum with Behavior (Strategy)
```java
enum Operation {
    ADD { int apply(int a, int b) { return a + b; } },
    SUBTRACT { int apply(int a, int b) { return a - b; } };

    abstract int apply(int a, int b);
}
```

## Use For
- Fixed set of constants
- State machines (OrderStatus, BookingState)
- Type-safe alternatives to strings
- Strategy pattern with limited options

## Avoid
- When values may change at runtime
- When set might grow frequently
- For open-ended categories
