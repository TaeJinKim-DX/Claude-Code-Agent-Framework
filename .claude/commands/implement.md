---
description: 구현 진행
argument-hint: [task]
---

Read `CLAUDE.md` and `AI_WORKFLOW.md` first.

Then use:
- `.opencode/agents/build.md`
- `docs/ai/runtime-python.md` or `docs/ai/runtime-typescript.md`

Implement:

$ARGUMENTS

Constraints:
- keep interface compatibility
- use LiteLLM Gateway
- update tests with code changes
- avoid unnecessary refactoring
- preserve logging and error handling
- keep side effects explicit

Output:
1. changed files
2. implementation summary
3. tests updated or added
4. remaining risks
