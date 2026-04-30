# Class Relationships

## Association
Objects know about each other, independent lifecycles.
```java
class Driver {
    private Car currentCar; // Can change, car exists independently
}
```

## Aggregation (has-a, weak)
Container holds references, parts exist independently.
```java
class Team {
    private List<Player> players; // Players can exist without team
}
```

## Composition (has-a, strong)
Container owns parts, parts die with container.
```java
class House {
    private final List<Room> rooms; // Rooms don't exist without house

    House() {
        rooms = List.of(new Room(), new Room());
    }
}
```

## Inheritance (is-a)
Use sparingly. Prefer composition.
```java
class Dog extends Animal { } // Dog IS an Animal
```

## Quick Decision
- Can part exist without whole? → Aggregation
- Part created/destroyed with whole? → Composition
- Is-a relationship? → Inheritance (but consider interface)
