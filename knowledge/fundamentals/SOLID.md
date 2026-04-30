# SOLID Principles

## S - Single Responsibility Principle

**Definition**: A class should have only one reason to change.

**Violation**:
```java
class UserService {
    void createUser(UserData data) { }
    void sendWelcomeEmail(User user) { }      // Email responsibility
    String generateReport(List<User> users) { } // Reporting responsibility
}
```

**Correct**:
```java
class UserService {
    void createUser(UserData data) { }
}

class UserEmailService {
    void sendWelcomeEmail(User user) { }
}

class UserReportService {
    String generateReport(List<User> users) { }
}
```

---

## O - Open/Closed Principle

**Definition**: Open for extension, closed for modification.

**Violation**:
```java
class DiscountCalculator {
    double calculate(Order order) {
        if (order.getType() == "regular") {
            return 0;
        } else if (order.getType() == "premium") {
            return order.getTotal() * 0.1;
        } else if (order.getType() == "vip") {  // Adding new type requires modification
            return order.getTotal() * 0.2;
        }
    }
}
```

**Correct**:
```java
interface DiscountStrategy {
    double calculate(Order order);
}

class RegularDiscount implements DiscountStrategy {
    public double calculate(Order order) { return 0; }
}

class PremiumDiscount implements DiscountStrategy {
    public double calculate(Order order) { return order.getTotal() * 0.1; }
}

// New types added by creating new classes, not modifying existing
class VipDiscount implements DiscountStrategy {
    public double calculate(Order order) { return order.getTotal() * 0.2; }
}
```

---

## L - Liskov Substitution Principle

**Definition**: Subtypes must be substitutable for their base types without altering correctness.

**Violation**:
```java
class Rectangle {
    protected int width, height;
    void setWidth(int w) { width = w; }
    void setHeight(int h) { height = h; }
    int getArea() { return width * height; }
}

class Square extends Rectangle {
    void setWidth(int w) { width = w; height = w; }  // Breaks expectations
    void setHeight(int h) { width = h; height = h; } // Breaks expectations
}

// This fails:
Rectangle r = new Square();
r.setWidth(5);
r.setHeight(10);
assert r.getArea() == 50; // Fails! Area is 100
```

**Correct**:
```java
interface Shape {
    int getArea();
}

class Rectangle implements Shape {
    private final int width, height;
    Rectangle(int w, int h) { width = w; height = h; }
    int getArea() { return width * height; }
}

class Square implements Shape {
    private final int side;
    Square(int s) { side = s; }
    int getArea() { return side * side; }
}
```

---

## I - Interface Segregation Principle

**Definition**: Clients should not depend on interfaces they don't use.

**Violation**:
```java
interface Worker {
    void work();
    void eat();
    void sleep();
}

class Robot implements Worker {
    void work() { /* ok */ }
    void eat() { throw new UnsupportedOperationException(); }  // Robots don't eat
    void sleep() { throw new UnsupportedOperationException(); } // Robots don't sleep
}
```

**Correct**:
```java
interface Workable {
    void work();
}

interface Feedable {
    void eat();
}

interface Restable {
    void sleep();
}

class Human implements Workable, Feedable, Restable {
    void work() { }
    void eat() { }
    void sleep() { }
}

class Robot implements Workable {
    void work() { }
}
```

---

## D - Dependency Inversion Principle

**Definition**: High-level modules should not depend on low-level modules. Both should depend on abstractions.

**Violation**:
```java
class MySQLDatabase {
    void save(String data) { }
}

class UserRepository {
    private MySQLDatabase database = new MySQLDatabase();  // Direct dependency on concrete class

    void saveUser(User user) {
        database.save(user.toString());
    }
}
```

**Correct**:
```java
interface Database {
    void save(String data);
}

class MySQLDatabase implements Database {
    public void save(String data) { }
}

class PostgresDatabase implements Database {
    public void save(String data) { }
}

class UserRepository {
    private final Database database;  // Depends on abstraction

    UserRepository(Database database) {  // Injected
        this.database = database;
    }

    void saveUser(User user) {
        database.save(user.toString());
    }
}
```
