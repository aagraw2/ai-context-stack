# AIContextStack

Structured knowledge for software design. **Shared rules live in `knowledge/`** — this file only routes Codex to them.

## Before any task

Read `knowledge/prompts/context-retrieval.md` and follow it. Navigate from `knowledge/_index.md`. Fetch capsules and summaries before full documents. Do not load entire folders.

Default to Java for code examples unless the user specifies otherwise.

## Workflows

When the user asks to **select context** (or pick knowledge files):

- Follow `.claude/commands/select-context.md`

When the user asks for **system design**:

- Follow `.claude/commands/system-design.md`

When the user asks for **code review**:

- Follow `.claude/commands/code-review.md`

When the user asks to **debug**:

- Follow `.claude/commands/debug.md`

When the user asks for **AI-assisted coding** help:

- Follow `.claude/commands/ai-coding.md`

## Knowledge layout

| Directory | Purpose |
| --------- | ------- |
| `knowledge/capsules/` | Atomic concepts |
| `knowledge/fundamentals/` | SOLID, clean code, errors |
| `knowledge/patterns/` | Design patterns |
| `knowledge/tradeoffs/` | Architecture decisions |
| `knowledge/anti-patterns/` | Pitfalls |
| `knowledge/case-studies/` | Examples |
| `knowledge/prompts/` | Task prompts and context-retrieval rules |

## Principles

Use `knowledge/fundamentals/_summary.md` and `knowledge/anti-patterns/_index.md` when reviewing or writing code. Prefer simple designs; justify tradeoffs.
