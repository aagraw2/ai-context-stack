# AIContextStack

A structured knowledge system to improve how LLMs consume context — with **one source of truth** in `knowledge/` and thin adapters per tool.

## Core idea

LLM quality depends on **which** context you provide, not how much. This repo organizes design knowledge into layers and selects only what a problem needs.

Benefits:

- Lower token usage
- Faster responses
- Better signal-to-noise
- Consistent outputs across Claude, Cursor, and OpenAI Codex

---

## Layout

```
knowledge/                 # Shared truth (content + retrieval rules)
  _index.md
  capsules/
  fundamentals/
  patterns/
  tradeoffs/
  anti-patterns/
  case-studies/
  prompts/
    context-retrieval.md   # How every tool should fetch knowledge

.claude/commands/          # Claude Code slash commands (orchestration only)
.cursor/rules/             # Cursor rules (pointers to knowledge/)
AGENTS.md                  # OpenAI Codex instructions
CLAUDE.md                  # Claude project pointer + command list
```

**Rule:** Put principles, prompts, and retrieval logic in `knowledge/`. Adapters only route agents to those files — no duplicated rulebooks.

---

## Tool matrix

| Tool | Auto-loaded | Workflows |
| ---- | ----------- | --------- |
| **Claude Code** | `CLAUDE.md` | `/select-context`, `/system-design`, `/code-review`, `/debug`, `/ai-coding` |
| **Cursor** | `.cursor/rules/*.mdc` | Same flows via chat; rules point at `knowledge/prompts/context-retrieval.md` |
| **OpenAI Codex** | `AGENTS.md` | Named workflows in `AGENTS.md` → `.claude/commands/*.md` |

---

## Flow

1. Start from the user's problem (e.g. design a notification system).
2. Follow `knowledge/prompts/context-retrieval.md` (or run `/select-context` in Claude).
3. Use `knowledge/_index.md` to pick 2–5 relevant files — capsules first.
4. Run the task prompt if needed (`system-design.md`, `code-review.md`, etc.).
5. Iterate only if a gap appears; do not load the whole knowledge base.

---

## Example

**Query:** Design a notification system with retries and idempotency.

**Selected context (illustrative):**

- `knowledge/prompts/context-retrieval.md` (rules)
- `knowledge/capsules/concurrency-basics.md`
- `knowledge/fundamentals/error-handling.md`
- `knowledge/tradeoffs/layered-architecture.md`
- `knowledge/prompts/system-design.md` (task format)

**Result:** Focused tradeoff analysis without loading all of `patterns/` or `case-studies/`.

---

## Knowledge layers

| Layer | Purpose |
| ----- | ------- |
| Fundamentals | SOLID, clean code, error handling |
| Capsules | Reusable atomic concepts |
| Patterns | Creational, structural, behavioral |
| Tradeoffs | Architecture decisions |
| Anti-patterns | Common pitfalls |
| Case studies | Real-world examples |
| Prompts | Task templates + context retrieval |

Indexes: each area has `_index.md` or `_summary.md`; root map is `knowledge/_index.md`.

---

## Extensibility

- Rule-based selection (current)
- Embedding search or vector DB (optional later)

---

## Why this matters

- **Separation:** storage (`knowledge/`) vs. tool wiring (`.claude/`, `.cursor/`, `AGENTS.md`)
- **Consistency:** same retrieval rules everywhere
- **Efficiency:** selective fetch as a first-class design choice
