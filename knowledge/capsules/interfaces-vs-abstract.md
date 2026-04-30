# Interface vs Abstract Class

## Interface
Contract only. No state. Multiple inheritance.
```java
interface Drawable {
    void draw();
}
interface Clickable {
    void onClick();
}
class Button implements Drawable, Clickable { }
```

## Abstract Class
Partial implementation. Has state. Single inheritance.
```java
abstract class Vehicle {
    protected int speed;

    void accelerate() { speed += 10; }
    abstract void honk(); // Subclass must implement
}
```

## Decision Guide

| Use Interface When | Use Abstract When |
|-------------------|-------------------|
| Defining capability | Sharing code + state |
| Multiple inheritance needed | Common base behavior |
| Unrelated classes share behavior | Is-a relationship exists |
| API contract | Template method pattern |

## Modern Java
Interfaces can have default methods, but avoid state.
```java
interface Sortable {
    default void sort() { /* default impl */ }
}
```
