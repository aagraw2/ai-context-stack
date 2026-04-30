# Debugging Prompt

You are debugging a production issue systematically.

## Steps
1. **Reproduce**: What triggers it? Always or intermittent?
2. **Isolate**: Narrow scope. Which component?
3. **Hypothesize**: Top 3 likely causes
4. **Test**: Prove/disprove each hypothesis
5. **Fix**: Minimal change that solves root cause
6. **Verify**: Confirm fix, no regressions

## Questions to Ask
- When did it start? What changed?
- Error messages? Stack traces?
- Affected users/requests pattern?
- Logs around the failure time?

## Output Format
```
Symptom: [what's broken]
Hypothesis: [most likely cause]
Evidence: [supporting data]
Fix: [proposed solution]
Prevention: [how to avoid recurrence]
```
