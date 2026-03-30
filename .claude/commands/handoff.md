# /handoff Command Specification

## File Contract
- Why it exists: 역할/세션 전환 시 작업 연속성을 보장하기 위해 존재한다.
- What belongs: 인수인계 스키마, 완료/미완료, 다음 액션.
- What does not belong: 신규 설계/구현 상세.
- Suggested section structure: Intent → Preconditions → Inputs → Protocol → Outputs → Validation → Failure → Stop.
- Typical mistakes: 블로커 누락.
- Preventive guidance: handoff-contract 필수 필드를 강제한다.

## Intent
역할 간 또는 세션 간 연속 작업을 위해 구조화된 인수인계를 생성한다.

## Preconditions / Preflight Checks
- 인수인계 대상 역할 지정
- 최신 산출물/증거 링크 수집

## Inputs
- Required: task-id, 대상 역할
- Optional: 우선 순위/주의사항

## Step-by-Step Execution Protocol
1. handoff-contract 스키마 채우기
2. 완료/미완료/블로커 분리
3. 다음 액션 단일화
4. 인수인계 파일 저장

## Output Artifacts
- `.sisyphus/drafts/<task-id>-handoff.md`

## Validation Checklist
- [ ] 필수 필드 누락 없음
- [ ] 증거 링크 유효
- [ ] 다음 액션이 1개 이상 명시됨
- [ ] handoff payload가 `.claude/agents/handoff-contract.md` 스키마와 일치
- [ ] `.claude/harness/acceptance-checklist.md` 인수인계 관련 항목 충족
- [ ] `.claude/harness/evidence-spec.md` 기준으로 evidence 링크 완전성 확인
- [ ] 이슈 발생 시 `.claude/harness/runbook.md` triage 경로 명시

## Failure Modes and Retry Protocol
- 실패: 링크 누락/상태 불명확
- 재시도: 누락 필드 보완 후 재발행

## Stop Conditions
- 수신 역할이 즉시 작업 재개 가능한 상태면 종료
