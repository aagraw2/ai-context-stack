# Error Handling Principles

## Use Exceptions for Exceptional Cases

**Definition**: Exceptions are for unexpected errors, not control flow.

**Violation**:
```java
// Using exceptions for control flow
try {
    int value = Integer.parseInt(input);
    process(value);
} catch (NumberFormatException e) {
    processAsString(input);  // Expected case handled via exception
}
```

**Correct**:
```java
if (isNumeric(input)) {
    process(Integer.parseInt(input));
} else {
    processAsString(input);
}
```

---

## Create Specific Exception Types

**Definition**: Use domain-specific exceptions that carry context.

**Violation**:
```java
throw new RuntimeException("User not found");
throw new RuntimeException("Invalid email format");
throw new RuntimeException("Duplicate entry");
```

**Correct**:
```java
class UserNotFoundException extends DomainException {
    UserNotFoundException(UserId id) {
        super("User not found: " + id);
    }
}

class InvalidEmailException extends ValidationException {
    InvalidEmailException(String email) {
        super("Invalid email format: " + email);
    }
}
```

---

## Don't Swallow Exceptions

**Violation**:
```java
try {
    riskyOperation();
} catch (Exception e) {
    // Silent failure - debugging nightmare
}
```

**Correct**:
```java
try {
    riskyOperation();
} catch (SpecificException e) {
    logger.error("Operation failed", e);
    throw new ServiceException("Could not complete operation", e);
}
```

---

## Use Result Types for Expected Failures

**Definition**: Use Result/Either types when failure is a normal outcome.

```java
sealed interface Result<T> {
    record Success<T>(T value) implements Result<T> {}
    record Failure<T>(Error error) implements Result<T> {}
}

class UserService {
    Result<User> findUser(UserId id) {
        return userRepository.findById(id)
            .map(Result.Success::new)
            .orElse(new Result.Failure<>(new UserNotFoundError(id)));
    }
}

// Usage
Result<User> result = userService.findUser(id);
switch (result) {
    case Result.Success<User> s -> display(s.value());
    case Result.Failure<User> f -> showError(f.error());
}
```

---

## Validate at Boundaries

**Definition**: Validate input at system boundaries, trust internal code.

```java
// API Controller - boundary, validate here
@PostMapping("/users")
Response createUser(@RequestBody UserRequest request) {
    ValidationResult validation = validator.validate(request);
    if (!validation.isValid()) {
        return Response.badRequest(validation.errors());
    }
    // After validation, internal code can trust the data
    User user = userService.create(request.toCommand());
    return Response.ok(user);
}

// Internal service - no need to re-validate
class UserService {
    User create(CreateUserCommand command) {
        // Trust that command is valid
        return repository.save(new User(command));
    }
}
```

---

## Provide Context in Errors

**Violation**:
```java
throw new IOException("File not found");
```

**Correct**:
```java
throw new FileNotFoundException(
    String.format("Configuration file not found: %s (searched in: %s)",
        filename,
        searchPaths)
);
```

---

## Clean Up Resources Properly

```java
// Use try-with-resources
try (Connection conn = dataSource.getConnection();
     PreparedStatement stmt = conn.prepareStatement(sql)) {
    return stmt.executeQuery();
}
// Resources automatically closed even on exception
```
