# Handoff Contract (Mandatory Schema)

## Required Fields
- task_id
- from_role
- to_role
- objective
- completed_items
- pending_items
- blockers
- changed_artifacts
- verification_results
- evidence_links
- risks
- next_action

## Schema Example

```yaml
task_id: TASK-2026-0001
from_role: planner
to_role: builder
objective: "표준 스켈레톤 문서 전환"
completed_items:
  - "범위 정의 완료"
pending_items:
  - "commands 문서 상세화"
blockers: []
changed_artifacts:
  - ".sisyphus/plans/TASK-2026-0001.md"
verification_results:
  diagnostics: "N/A"
  tests: "N/A"
evidence_links:
  - ".sisyphus/evidence/TASK-2026-0001/plan-proof.md"
risks:
  - "legacy 문서 참조 잔존 가능성"
next_action: "builder가 /implement 단계 시작"
```
