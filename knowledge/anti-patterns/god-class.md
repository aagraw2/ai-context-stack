# Anti-Pattern: God Class

## Symptoms
- Class > 500 lines
- 10+ dependencies
- Touches multiple domains
- "Manager", "Handler", "Processor" names

## Examples

**Bad**: UserManager does everything
```java
class UserManager {
    void createUser() { }
    void sendEmail() { }
    void processPayment() { }
    void generateReport() { }
    void syncToExternalSystem() { }
    void validateAddress() { }
    // 2000 lines...
}
```

**Good**: Single responsibility
```java
class UserService { void create() { } }
class UserEmailService { void sendWelcome() { } }
class UserBillingService { void charge() { } }
class UserReportService { void generate() { } }
```

## Root Causes
- Organic growth without refactoring
- Unclear ownership
- Fear of creating "too many" classes

## Fix
- Extract class by responsibility
- One reason to change per class
- Identify clusters of related methods
- Use domain events for cross-cutting
