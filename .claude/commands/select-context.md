Select the minimal knowledge files for the problem in the **user's message**.

Read and follow:
- `knowledge/prompts/context-retrieval.md`
- `knowledge/_index.md`

## Output

1. **Problem classification** (one line)
2. **Files to fetch** — bullet list of paths under `knowledge/` only (typically 2–5 files)
3. **Rationale** — one short sentence per file

Do **not** load full document contents unless the user asks to proceed with the task. Do **not** fetch entire folders.
