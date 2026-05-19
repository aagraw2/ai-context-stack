# Context Retrieval

Shared rules for selecting knowledge from this repository. All tools (Claude, Cursor, Codex) should follow this document.

## General rule

Always prefer **selective context retrieval** over loading large amounts of information.

## Knowledge base structure

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

Start navigation at `knowledge/_index.md`.

## Step-by-step retrieval

1. Read `knowledge/_index.md` to find the right area
2. Open the relevant `_index.md` or `_summary.md` in that area
3. Fetch **capsules** first (small, high-signal)
4. Fetch **full documents** only when summaries are insufficient

## When mapping is unclear

1. Classify the problem:
   - System Design
   - Low Level Design
   - Debugging / Code Review
   - Data Modeling
   - Performance / Concurrency
2. Use `knowledge/_index.md` to identify relevant areas
3. Select **1–2 capsules** and at most **one** summary or tradeoff doc
4. Avoid over-fetching

## Topic → file mapping

| Topic | Files to fetch |
| ----- | -------------- |
| SOLID principles | `fundamentals/_summary.md` → `fundamentals/SOLID.md` |
| Clean code / error handling | `fundamentals/_summary.md` → specific file |
| Design patterns | `patterns/_summary.md` → specific pattern file |
| Object creation | `capsules/builder-vs-factory.md` |
| Class relationships | `capsules/class-relationships.md` |
| Interface vs abstract | `capsules/interfaces-vs-abstract.md` |
| Domain modeling | `capsules/entity-vs-value.md` |
| Collections / enums | `capsules/collections.md`, `capsules/enums.md` |
| Concurrency | `capsules/concurrency-basics.md`, `capsules/immutability.md` |
| Architecture decisions | `tradeoffs/_index.md` → specific file |
| Anti-patterns | `anti-patterns/_index.md` → specific file |
| Case studies | `case-studies/_index.md` → specific file |
| Code review | `prompts/code-review.md` |
| System design | `prompts/system-design.md` |
| Debugging | `prompts/debugging.md` |
| AI coding | `prompts/ai-coding.md` |

## Fetch priority (this repo)

1. **Capsules** — minimal, high-signal
2. **Summary files** — quick understanding
3. **Full documents** — only when deeper detail is required

## Do NOT fetch

- Multiple full documents at once
- Entire folders
- Unrelated topics
- Extra context "just in case"

## Approach (design and implementation)

1. Clarify requirements
2. Identify entities and relationships
3. Define interfaces before implementations
4. Use the knowledge base for principles and patterns
5. Justify design decisions with tradeoffs
6. Keep solutions simple and extensible

## When editing application code

Prefer reading in this order:

1. Interface definitions
2. Domain models / entities
3. Existing patterns in the codebase
4. Tests for expected behavior
5. Implementation details last

Do not load entire files when only one method is needed; skip unrelated modules and generated code.

## Language

Default to Java for code examples unless specified otherwise.
