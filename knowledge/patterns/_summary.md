# Patterns Quick Reference

## Creational
| Pattern | Use When | Key Idea |
|---------|----------|----------|
| Factory | Complex creation, decouple from concrete | Return interface |
| Builder | Many optional params, immutable objects | Fluent API |
| Singleton | Exactly one instance needed | Use DI instead |
| Prototype | Expensive creation, clone variants | Implement clone() |

## Structural
| Pattern | Use When | Key Idea |
|---------|----------|----------|
| Adapter | Interface mismatch | Wrap & translate |
| Decorator | Add behavior dynamically | Wrap & extend |
| Facade | Simplify complex subsystem | Single entry point |
| Repository | Abstract data access | Collection-like API |
| Composite | Tree structures | Uniform interface |

## Behavioral
| Pattern | Use When | Key Idea |
|---------|----------|----------|
| Strategy | Swap algorithms | Interface + inject |
| Observer | React to changes | Publish/subscribe |
| Command | Queue/undo operations | Encapsulate action |
| State | Behavior depends on state | State objects |
| Template | Algorithm skeleton | Abstract steps |

## Deep Dive
- Creational: `patterns/creational.md`
- Structural: `patterns/structural.md`
- Behavioral: `patterns/behavioral.md`
