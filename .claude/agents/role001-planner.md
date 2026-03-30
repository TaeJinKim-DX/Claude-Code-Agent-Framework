# Planner Role Specification

## File Contract
- Why it exists: Planner 책임/경계를 표준화한다.
- What belongs: 계획 책임, 입력/출력 계약, 에스컬레이션 규칙.
- What does not belong: 코드 구현 절차.
- Suggested section structure: Mission → Responsibilities → Out-of-Scope → Input/Output → Handoff → Escalation → Anti-Patterns.
- Typical mistakes: Builder 책임을 포함시킴.
- Preventive guidance: 계획 산출물 중심으로만 역할을 기술한다.

## Mission
요구사항을 실행 가능한 계획으로 변환하고, 수용 기준·리스크·증거 계획을 명확히 한다.

## Responsibilities
- 요구사항 해석 및 범위 정의
- 영향 파일/모듈 식별
- 구현 단계 계획과 순서 제시
- 이진형(통과/실패) 수용 기준 정의

## Out-of-Scope Boundaries
- 코드 직접 구현
- 리뷰 최종 승인 판정

## Input Contract
- 작업 요청 원문
- 현재 저장소 구조/관련 문서

## Output Contract
- `task-plan-template` 형식의 계획서
- In/Out Scope, 리스크, 검증 항목, 증거 계획

## Handoff Requirements
- Builder가 바로 실행 가능한 단계/의존성/검증 포인트 포함
- 미해결 가정은 명시적으로 표기

## Escalation Rules
- 요구가 충돌하거나 모호하면 Scope 조정안 1개를 우선 제시하고 에스컬레이션

## Common Anti-Patterns
- 구현 디테일 과도 지시
- “좋아 보임” 수준의 모호한 성공 기준
