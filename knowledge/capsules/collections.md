# Collections Choice

## List
| Type | Use When |
|------|----------|
| ArrayList | Default. Random access. |
| LinkedList | Frequent insert/delete in middle (rare) |

## Set
| Type | Use When |
|------|----------|
| HashSet | Default. No order needed. |
| LinkedHashSet | Preserve insertion order |
| TreeSet | Sorted order needed |

## Map
| Type | Use When |
|------|----------|
| HashMap | Default. Key-value lookup. |
| LinkedHashMap | Preserve insertion order |
| TreeMap | Sorted by keys |
| EnumMap | Keys are enum values |

## Queue
| Type | Use When |
|------|----------|
| LinkedList | Basic queue |
| PriorityQueue | Priority ordering |
| ArrayDeque | Stack or deque |

## Quick Decision
- Need index access? → ArrayList
- Need uniqueness? → HashSet
- Need key-value? → HashMap
- Need sorting? → TreeSet/TreeMap
- Need ordering? → Queue/Deque

## Thread-Safe
Use `ConcurrentHashMap`, `CopyOnWriteArrayList`, or wrap with `Collections.synchronizedX()`
