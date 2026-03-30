# quality-gates.md

## Why it exists
Plan/Build/Review 수명주기의 명확한 Definition of Done을 제공한다.

## Definition of Done

### Plan DoD
- 범위(In/Out) 명시
- 이진 수용 기준 명시
- 리스크/완화/증거 계획 명시

### Build DoD
- 계획 범위 내 구현 완료
- 진단/테스트/수동 QA 결과 기록
- 구현 로그와 증거 링크 작성

### Review DoD
- scope/quality/evidence gate 판정 완료
- verdict 명시(APPROVED/CHANGES_REQUIRED/BLOCKED)

## Pass/Fail Checks
- Pass: 모든 단계 DoD 충족
- Fail: 하나라도 미충족

## Harness Binding (Enforceable)
- `/start-work`, `/plan`, `/implement`, `/review`, `/handoff`는 아래 하네스 문서를 강제 참조한다.
  - `.claude/harness/acceptance-checklist.md`
  - `.claude/harness/evidence-spec.md`
  - `.claude/harness/runbook.md`
- 위 3개 문서 중 하나라도 누락/불일치하면 Review verdict는 `APPROVED`가 될 수 없다.

## Typical mistakes
- 수동 QA 누락

## Preventive guidance
- 하네스 체크리스트와 항상 동기화한다.
