---
name: Git workflow
description: apply when committing or branching — naming, format, split by module type, conventional prefixes
type: feedback
---

**Always**
- Branch: `type/short-description` kebab-case — e.g. `feat/add-completion`, `fix/parse-error`
- Stage: `git add <explicit files>`; never `git add .` or `-A`
- Split: one commit per module type — code, tests, docs, bash/helper scripts each separate
- Message: `type: short description` using the most semantically accurate prefix:
  `feat` `fix` `perf` `refactor` `test` `docs` `style` `chore` `ci` `build`
- Never: `--no-verify`, `--force`, skip hooks, add Co-Authored-By — unless user explicitly asks
