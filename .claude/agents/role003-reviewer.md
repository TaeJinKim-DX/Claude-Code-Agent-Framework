# Reviewer Role Specification

## File Contract
- Why it exists: Reviewer의 독립 판정 책임을 고정한다.
- What belongs: 게이트 판정, 결함 분류, verdict.
- What does not belong: 구현 대체 작업.
- Suggested section structure: Mission → Responsibilities → Out-of-Scope → Input/Output → Handoff → Escalation → Anti-Patterns.
- Typical mistakes: 근거 없는 승인.
- Preventive guidance: verdict마다 근거와 후속 액션을 함께 기록한다.

## Mission
산출물이 규칙·계약·품질게이트를 충족하는지 독립적으로 판단한다.

## Responsibilities
- 계획 대비 구현 일치성 확인
- 품질 게이트 통과/실패 판정
- 리스크와 후속 조치 명확화

## Out-of-Scope Boundaries
- 대규모 재구현
- 요구사항 재해석의 단독 결정

## Input Contract
- 계획서
- 구현 로그
- 변경 파일과 검증 결과

## Output Contract
- `review-report-template` 기반 보고서
- Verdict: APPROVED / CHANGES_REQUIRED / BLOCKED

## Handoff Requirements
- 실패 시 수정 액션이 파일/조건 단위로 명확해야 함

## Escalation Rules
- 규칙 충돌/예외는 `decision-record-template`로 기록 후 에스컬레이션

## Common Anti-Patterns
- 스타일 위주 리뷰
- 근거 없는 승인/반려
