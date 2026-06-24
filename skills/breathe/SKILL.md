---
name: breathe
description: Enter plan mode, check git logs/diffs, and ask the user questions about their intended use case. Invoke when the user has repeatedly asked for a feature to be fixed and nothing seems to be working.
---

Enter plan mode and take a deliberate pause before doing anything else. The intention here is to "breathe" — stop wasting tokens on blind attempts and clearly direct development efforts.

## Steps

1. **Summarize what's been tried** — Run `git diff` and `git log --oneline -10` to see what has changed recently. List the failed approaches concisely so neither you nor the user has to re-explain them.

2. **Surface errors** — If there are known test failures, build errors, or runtime issues, state them clearly and concisely. Don't re-discover what's already known.

3. **List assumptions** — Before asking questions, list ALL your assumptions about the visual design, intention, and execution plan in a bulleted list with a percentage confidence value for each. This makes misalignments visible immediately.

4. **Ask probing questions** — Ask targeted questions to close the gaps. Prioritize:
   - "What's the smallest version of this that would be acceptable?" (scope reduction)
   - "Which of these failed approaches was closest to what you want?"
   - "Is there a constraint I'm missing?"

5. **Provide a context summary** — Output a compact, copy-pasteable summary of the current state: goal, what's been tried, current blockers, and agreed next step. This can be used to continue in a fresh conversation if the current one is too polluted to recover cheaply.
