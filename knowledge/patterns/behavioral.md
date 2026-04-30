# Behavioral Design Patterns

## Strategy Pattern

**Intent**: Define family of algorithms, encapsulate each, make them interchangeable.

**When to Use**:
- Multiple algorithms for same task
- Algorithm selection at runtime
- Avoid conditional statements for algorithm selection

**Structure**:
```
interface Strategy {
    execute(data)
}

class Context {
    strategy: Strategy
    doWork() { strategy.execute() }
}
```

**Example**:
```java
interface PricingStrategy {
    Money calculatePrice(Order order);
}

class RegularPricing implements PricingStrategy {
    public Money calculatePrice(Order order) {
        return order.getSubtotal();
    }
}

class PremiumPricing implements PricingStrategy {
    public Money calculatePrice(Order order) {
        return order.getSubtotal().multiply(0.9); // 10% discount
    }
}

class OrderService {
    public Money checkout(Order order, PricingStrategy pricing) {
        return pricing.calculatePrice(order);
    }
}
```

---

## Observer Pattern

**Intent**: Define one-to-many dependency. When one object changes, dependents are notified.

**When to Use**:
- State changes need to trigger updates elsewhere
- Loose coupling between subject and observers
- Event-driven architecture

**Structure**:
```
interface Observer {
    update(event)
}

class Subject {
    observers: List<Observer>
    attach(observer)
    detach(observer)
    notify() { observers.forEach(o -> o.update()) }
}
```

**Example**:
```java
interface OrderEventListener {
    void onOrderPlaced(OrderPlacedEvent event);
}

class OrderService {
    private final List<OrderEventListener> listeners;

    public void placeOrder(Order order) {
        // ... process order
        listeners.forEach(l -> l.onOrderPlaced(new OrderPlacedEvent(order)));
    }
}

class InventoryListener implements OrderEventListener {
    public void onOrderPlaced(OrderPlacedEvent event) {
        inventory.reserve(event.getItems());
    }
}

class EmailListener implements OrderEventListener {
    public void onOrderPlaced(OrderPlacedEvent event) {
        emailService.sendConfirmation(event.getEmail());
    }
}
```

---

## Command Pattern

**Intent**: Encapsulate request as object. Parameterize clients with different requests, queue or log requests.

**When to Use**:
- Parameterize objects with operations
- Queue, schedule, or log operations
- Support undo/redo functionality

**Example**:
```java
interface Command {
    void execute();
    void undo();
}

class CreateUserCommand implements Command {
    private final UserRepository repository;
    private final UserData data;
    private User createdUser;

    public void execute() {
        createdUser = repository.save(new User(data));
    }

    public void undo() {
        repository.delete(createdUser.getId());
    }
}

class CommandInvoker {
    private final Deque<Command> history = new ArrayDeque<>();

    public void execute(Command command) {
        command.execute();
        history.push(command);
    }

    public void undo() {
        if (!history.isEmpty()) {
            history.pop().undo();
        }
    }
}
```

---

## State Pattern

**Intent**: Allow object to alter behavior when internal state changes. Object appears to change class.

**When to Use**:
- Object behavior depends on state
- Complex conditional logic based on state
- State transitions are explicit

**Example**:
```java
interface OrderState {
    void next(OrderContext context);
    void cancel(OrderContext context);
}

class PendingState implements OrderState {
    public void next(OrderContext ctx) {
        ctx.setState(new ProcessingState());
    }
    public void cancel(OrderContext ctx) {
        ctx.setState(new CancelledState());
    }
}

class ProcessingState implements OrderState {
    public void next(OrderContext ctx) {
        ctx.setState(new ShippedState());
    }
    public void cancel(OrderContext ctx) {
        throw new IllegalStateException("Cannot cancel processing order");
    }
}

class OrderContext {
    private OrderState state = new PendingState();

    public void nextState() { state.next(this); }
    public void cancel() { state.cancel(this); }
}
```

---

## Template Method Pattern

**Intent**: Define skeleton of algorithm, defer some steps to subclasses.

**When to Use**:
- Common algorithm structure with varying steps
- Avoid code duplication in similar algorithms
- Control extension points

**Example**:
```java
abstract class DataExporter {
    // Template method
    public final void export(Data data) {
        connect();
        String formatted = format(data);
        write(formatted);
        disconnect();
    }

    protected abstract void connect();
    protected abstract String format(Data data);
    protected abstract void write(String data);
    protected abstract void disconnect();
}

class CsvExporter extends DataExporter {
    protected String format(Data data) {
        return data.toCsv();
    }
    // ... other implementations
}
```
