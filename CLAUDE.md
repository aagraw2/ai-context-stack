# AIContextStack

Structured knowledge for software design. **Shared rules live in `knowledge/`** — read `knowledge/prompts/context-retrieval.md` before selecting or loading files.

## Slash commands

| Command | Purpose |
| ------- | ------- |
| `/select-context` | Minimal file list for the user's problem |
| `/system-design` | System design review with selective knowledge fetch |
| `/code-review` | Code review using `knowledge/prompts/code-review.md` |
| `/debug` | Debugging using `knowledge/prompts/debugging.md` |
| `/ai-coding` | AI pair-programming using `knowledge/prompts/ai-coding.md` |

Command definitions: `.claude/commands/`

## Entry point

Start at `knowledge/_index.md`. Default language for examples: Java.
