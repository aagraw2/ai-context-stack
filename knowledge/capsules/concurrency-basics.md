# Concurrency Basics

## Thread Safety Options

**1. Immutability** (preferred)
```java
record Ticket(String id, String status) { }
```

**2. Synchronization**
```java
class Counter {
    private int count;
    public synchronized void increment() { count++; }
}
```

**3. Locks**
```java
class ParkingLot {
    private final Lock lock = new ReentrantLock();

    public boolean park(Vehicle v) {
        lock.lock();
        try {
            // critical section
        } finally {
            lock.unlock();
        }
    }
}
```

**4. Atomic Classes**
```java
private AtomicInteger available = new AtomicInteger(100);
available.decrementAndGet();
```

## Common Scenarios
| Problem | Solution |
|---------|----------|
| Counter | AtomicInteger |
| Shared resource | synchronized or Lock |
| Read-heavy | ReadWriteLock |
| Producer-consumer | BlockingQueue |

## Red Flags
- Mutable shared state without protection
- Nested locks (deadlock risk)
- Long-held locks
