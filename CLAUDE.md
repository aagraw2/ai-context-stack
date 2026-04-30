# Claude Context

## Project Purpose

This repository is a structured knowledge system for software design, architecture, and coding principles. It is optimized to provide **relevant, minimal context** to improve LLM response quality while reducing token usage.

---

## Knowledge Base Structure

```
knowledge/
├── fundamentals/      # Core principles (SOLID, clean code, error handling)
├── capsules/          # Atomic concepts (small, focused, reusable)
├── patterns/          # Design patterns (creational, structural, behavioral)
├── tradeoffs/         # Architecture and design decisions
├── anti-patterns/     # Common mistakes and failure modes
├── case-studies/      # Real-world applications
└── prompts/           # Prompt templates for different tasks
```

---

## Context Selection Strategy

### General Rule

Always prefer **selective context retrieval** over loading large amounts of information.

---

### Step-by-step Retrieval

1. **Start with** `knowledge/_index.md` to understand where to look
2. **Navigate to** relevant `_index.md` or `_summary.md` files
3. **Fetch capsules first** (small, high-signal)
4. **Fetch full documents only if necessary**

---

### When Mapping Is Unclear

If the query does not directly match a known topic:

1. Classify the problem into one of:

   * System Design
   * Low Level Design
   * Debugging / Code Review
   * Data Modeling
   * Performance / Concurrency

2. Use `knowledge/_index.md` to identify relevant areas

3. Select:

   * 1–2 capsules
   * 1 summary or tradeoff document (if needed)

4. Avoid over-fetching context

---

## Topic to File Mapping

| Topic                       | Files to Fetch                                               |
| --------------------------- | ------------------------------------------------------------ |
| SOLID principles            | `fundamentals/_summary.md` → `fundamentals/SOLID.md`         |
| Clean code / error handling | `fundamentals/_summary.md` → specific file                   |
| Design patterns             | `patterns/_summary.md` → specific pattern file               |
| Object creation             | `capsules/builder-vs-factory.md`                             |
| Class relationships         | `capsules/class-relationships.md`                            |
| Interface vs abstract       | `capsules/interfaces-vs-abstract.md`                         |
| Domain modeling             | `capsules/entity-vs-value.md`                                |
| Collections / enums         | `capsules/collections.md`, `capsules/enums.md`               |
| Concurrency                 | `capsules/concurrency-basics.md`, `capsules/immutability.md` |
| Architecture decisions      | `tradeoffs/_index.md` → specific file                        |
| Anti-patterns               | `anti-patterns/_index.md` → specific file                    |
| Case studies                | `case-studies/_index.md` → specific file                     |
| Code review                 | `prompts/code-review.md`                                     |
| System design               | `prompts/system-design.md`                                   |
| Debugging                   | `prompts/debugging.md`                                       |
| AI coding                   | `prompts/ai-coding.md`                                       |

---

## Fetch Priority

1. **Capsules** → minimal, high-signal, always safe
2. **Summary files** → quick understanding
3. **Full documents** → only when deeper detail is required

---

## Do NOT Fetch

* Multiple full documents at once
* Entire folders
* Unrelated topics
* Extra context "just in case"

---

## Approach

When solving problems:

1. Clarify requirements
2. Identify entities and relationships
3. Define interfaces before implementations
4. Use knowledge base for principles and patterns
5. Justify design decisions with tradeoffs
6. Keep solutions simple and extensible

---

## Language

Default to Java for code examples unless specified otherwise.
