# Harness Runbook

## Failure Triage Flow
1. 실패 유형 분류(산출물 누락/검증 실패/리뷰 실패)
2. 책임 역할 지정(Planner/Builder/Reviewer)
3. 재시도 경로 적용
4. evidence 갱신

## Ownership
- 계획 누락: Planner
- 구현 검증 누락: Builder
- 게이트 판정 누락: Reviewer

## Recovery Rule
- 모든 재시도는 동일 task-id의 evidence에 누적 기록한다.
