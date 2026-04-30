# Anti-Pattern: Tight Coupling

## Symptoms
- Changing one module breaks others
- Can't test without full system
- Circular dependencies
- God classes knowing everything

## Examples

**Bad**: Direct dependencies
```java
class OrderService {
    private MySQLOrderRepository repo = new MySQLOrderRepository();
    private StripePayment payment = new StripePayment();
    private SendGridEmail email = new SendGridEmail();
}
// Can't test, can't swap implementations
```

**Good**: Depend on abstractions
```java
class OrderService {
    private final OrderRepository repo;
    private final PaymentGateway payment;
    private final EmailService email;

    OrderService(OrderRepository r, PaymentGateway p, EmailService e) {
        this.repo = r; this.payment = p; this.email = e;
    }
}
```

## Root Causes
- "Just get it working"
- Skipping interface design
- Unclear module boundaries
- No dependency direction rules

## Fix
- Dependency inversion principle
- Define clear interfaces
- Constructor injection
- Enforce layer boundaries (ArchUnit)
