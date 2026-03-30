# /implement Command Specification

## File Contract
- Why it exists: 계획을 안전하게 실행하고 검증 증거를 남기기 위해 존재한다.
- What belongs: 구현 단계, 검증 실행, 로그 산출물.
- What does not belong: 요구 재정의, 최종 승인 판정.
- Suggested section structure: Intent → Preconditions → Inputs → Protocol → Outputs → Validation → Failure → Stop.
- Typical mistakes: 수동 QA 없이 종료.
- Preventive guidance: diagnostics/tests/manual QA를 모두 기록한다.

## Intent
승인된 계획 범위 내에서 구현을 수행하고 증거를 남긴다.

## Preconditions / Preflight Checks
- 최신 계획서 존재
- 소유권/범위 규칙 위반 없음

## Inputs
- Required: 계획서 경로
- Optional: 우선순위, 시간 제약

## Step-by-Step Execution Protocol
1. 계획 단계별 구현
2. 변경 근거 기록
3. 진단/테스트/수동 QA 실행
4. 구현 로그 작성

## Output Artifacts
- 코드/문서 변경
- `.sisyphus/drafts/<task-id>-implementation.md`

## Validation Checklist
- [ ] 계획 범위 내 변경
- [ ] 검증 결과 기록
- [ ] 남은 리스크 기재
- [ ] `.claude/harness/evidence-spec.md`의 필수 evidence 필드 충족
- [ ] `.claude/harness/acceptance-checklist.md` 구현 단계 항목 충족
- [ ] 실패 발생 시 `.claude/harness/runbook.md` triage 절차를 로그에 반영

## Failure Modes and Retry Protocol
- 실패: 테스트/검증 실패
- 재시도: 원인 수정 후 동일 검증 재실행

## Stop Conditions
- Reviewer가 검토 가능한 증거 세트 준비 시 종료
