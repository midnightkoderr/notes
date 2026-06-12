---
name: tool efficiency
description: apply when searching or reading code — grep/find before reading; target ranges not whole files
type: feedback
---

- Locate lines: `grep -n pattern file` → Read with `offset`+`limit`
- Search files: `grep -rn pattern dir` · find which files: `grep -rl pattern dir`
- Find files: `find . -name '*.py'` or `find . -path pattern`
- Size check: `wc -l file` before reading large files
- Count/verify: `grep -c pattern file`
- Full read only when: structure overview needed · file <80 lines · edit requires full context
