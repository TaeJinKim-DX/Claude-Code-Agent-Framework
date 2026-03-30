# /plan Command Specification

## File Contract
- Why it exists: 요구를 실행 가능한 계획으로 고정하기 위해 존재한다.
- What belongs: 범위 정의, 리스크, 수용 기준, 증거 계획.
- What does not belong: 구현 코드, 리뷰 verdict.
- Suggested section structure: Intent → Preconditions → Inputs → Protocol → Outputs → Validation → Failure → Stop.
- Typical mistakes: 수용 기준을 모호하게 적음.
- Preventive guidance: 모든 기준을 pass/fail 문장으로 작성한다.

## Intent
요구사항을 실행 가능한 계획과 이진 수용 기준으로 변환한다.

## Preconditions / Preflight Checks
- `/start-work` 산출물 확인
- 규칙 문서(`ownership`, `scope-control`, `quality-gates`) 확인

## Inputs
- Required: 작업 설명, 제약 조건
- Optional: 관련 파일 경로, 참조 이슈

## Step-by-Step Execution Protocol
1. 목적/완료조건 정의
2. In Scope / Out of Scope 확정
3. 영향 파일과 리스크 식별
4. 단계별 실행 계획 작성
5. 수용 기준과 증거 계획 작성

## Output Artifacts
- `.sisyphus/plans/<task-id>.md` (`task-plan-template` 준수)

## Validation Checklist
- [ ] 수용 기준이 pass/fail 형태
- [ ] 범위 외 항목이 명시됨
- [ ] 리스크/완화 전략 존재
- [ ] 하네스 기준(`.claude/harness/acceptance-checklist.md`)의 계획 산출물 조건 충족
- [ ] `.claude/harness/evidence-spec.md`에 맞는 증거 계획 필드 포함
- [ ] 실패 시 `.claude/harness/runbook.md` triage 경로 반영

## Failure Modes and Retry Protocol
- 실패: 모호한 요구로 계획 불완전
- 재시도: 범위 최소화 해석으로 계획 재작성

## Stop Conditions
- Builder가 구현 착수 가능한 수준의 계획 완성 시 종료
