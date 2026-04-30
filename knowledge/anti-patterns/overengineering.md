# Anti-Pattern: Overengineering

## Symptoms
- Abstractions for things that never vary
- Factories for single implementations
- Microservices for 100 users
- Config files for hardcoded values

## Examples

**Bad**: Generic everything
```java
interface MessageProcessor<T extends Message, R extends Result> {
    R process(T message, Context ctx, Options opts);
}
// Used exactly once with String->String
```

**Good**: Direct solution
```java
String processMessage(String message) { ... }
// Add abstraction when second use case appears
```

## Root Causes
- Predicting future requirements
- Resume-driven development
- Fear of refactoring later
- Copying enterprise patterns blindly

## Fix
- YAGNI: Build for today's requirements
- Rule of 3: Abstract after 3 duplications
- Prefer boring technology
- Refactoring is cheaper than wrong abstraction
