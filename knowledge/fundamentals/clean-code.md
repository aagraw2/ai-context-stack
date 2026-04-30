# Clean Code Principles

## DRY - Don't Repeat Yourself

**Definition**: Every piece of knowledge must have a single, unambiguous representation.

**Violation**:
```java
class OrderService {
    void processOrder(Order order) {
        double tax = order.getSubtotal() * 0.08;
        double total = order.getSubtotal() + tax;
        // ...
    }

    void previewOrder(Order order) {
        double tax = order.getSubtotal() * 0.08;  // Duplicated logic
        double total = order.getSubtotal() + tax;
        // ...
    }
}
```

**Correct**:
```java
class TaxCalculator {
    private static final double TAX_RATE = 0.08;

    Money calculateTax(Money subtotal) {
        return subtotal.multiply(TAX_RATE);
    }

    Money calculateTotal(Money subtotal) {
        return subtotal.add(calculateTax(subtotal));
    }
}
```

---

## KISS - Keep It Simple, Stupid

**Definition**: Prefer simple solutions over complex ones.

**Overcomplicated**:
```java
class StringReverser {
    public String reverse(String input) {
        return IntStream.range(0, input.length())
            .mapToObj(i -> input.charAt(input.length() - 1 - i))
            .collect(StringBuilder::new, StringBuilder::append, StringBuilder::append)
            .toString();
    }
}
```

**Simple**:
```java
class StringReverser {
    public String reverse(String input) {
        return new StringBuilder(input).reverse().toString();
    }
}
```

---

## YAGNI - You Aren't Gonna Need It

**Definition**: Don't add functionality until it's necessary.

**Violation**:
```java
class User {
    private String name;
    private String email;
    private String futureField1;        // "Might need later"
    private String futureField2;        // "Might need later"
    private Map<String, Object> metadata; // "For extensibility"

    // Getters/setters for fields that may never be used
}
```

**Correct**:
```java
class User {
    private String name;
    private String email;
    // Add fields when actually needed
}
```

---

## Composition Over Inheritance

**Definition**: Favor object composition over class inheritance for code reuse.

**Problematic Inheritance**:
```java
class ArrayList<T> { }
class Stack<T> extends ArrayList<T> {
    // Inherits all ArrayList methods including ones that violate stack semantics
    // like add(index, element), remove(index)
}
```

**Composition**:
```java
class Stack<T> {
    private final List<T> items = new ArrayList<>();

    void push(T item) { items.add(item); }
    T pop() { return items.remove(items.size() - 1); }
    T peek() { return items.get(items.size() - 1); }
    boolean isEmpty() { return items.isEmpty(); }
    // Only exposes stack operations
}
```

---

## Program to Interfaces

**Definition**: Depend on abstractions, not concrete implementations.

**Violation**:
```java
class ReportGenerator {
    private ArrayList<Data> dataList;  // Concrete type

    void setData(ArrayList<Data> data) {
        this.dataList = data;
    }
}
```

**Correct**:
```java
class ReportGenerator {
    private List<Data> dataList;  // Interface type

    void setData(List<Data> data) {
        this.dataList = data;
    }
}
```

---

## Fail Fast

**Definition**: Detect and report errors as early as possible.

**Violation**:
```java
void processUser(User user) {
    // Continues with null, fails later with confusing error
    String email = user != null ? user.getEmail() : null;
    // ... 50 lines later
    sendEmail(email);  // NullPointerException here - hard to trace
}
```

**Correct**:
```java
void processUser(User user) {
    Objects.requireNonNull(user, "User cannot be null");
    Objects.requireNonNull(user.getEmail(), "User email cannot be null");

    sendEmail(user.getEmail());
}
```

---

## Law of Demeter

**Definition**: Only talk to immediate friends. Don't reach through objects.

**Violation**:
```java
// Train wreck - reaching through multiple objects
String city = order.getCustomer().getAddress().getCity();
```

**Correct**:
```java
// Ask, don't reach
String city = order.getShippingCity();

// Inside Order class:
String getShippingCity() {
    return customer.getShippingCity();
}
```

---

## Single Level of Abstraction

**Definition**: Each method should operate at a single level of abstraction.

**Violation**:
```java
void processOrder(Order order) {
    // High level
    validateOrder(order);

    // Low level details mixed in
    Connection conn = DriverManager.getConnection(url);
    PreparedStatement stmt = conn.prepareStatement("INSERT INTO orders...");
    stmt.setString(1, order.getId());
    stmt.executeUpdate();

    // High level again
    sendConfirmation(order);
}
```

**Correct**:
```java
void processOrder(Order order) {
    validateOrder(order);
    saveOrder(order);
    sendConfirmation(order);
}

private void saveOrder(Order order) {
    orderRepository.save(order);
}
```
