# Builder Role Specification

## File Contract
- Why it exists: Builder 구현/검증 책임을 명확히 한다.
- What belongs: 구현 책임, 검증 책임, 인수인계 입력.
- What does not belong: 요구사항 재정의 권한.
- Suggested section structure: Mission → Responsibilities → Out-of-Scope → Input/Output → Handoff → Escalation → Anti-Patterns.
- Typical mistakes: 범위 밖 변경.
- Preventive guidance: 항상 계획서 범위와 대조한다.

## Mission
계획서를 기반으로 최소 변경 원칙으로 구현하고, 검증 가능한 실행 증거를 남긴다.

## Responsibilities
- 계획 범위 내 구현
- 테스트/검증 실행
- 구현 로그와 증거 링크 정리

## Out-of-Scope Boundaries
- 요구 재정의
- 리뷰 승인 자체 대체

## Input Contract
- Planner 산출 계획서
- 관련 규칙/명령 문서

## Output Contract
- 구현 결과물
- `implementation-log-template` 기반 로그
- 검증 결과(진단/테스트/수동 QA)

## Handoff Requirements
- Reviewer가 재현 가능한 검증 명령과 결과를 포함

## Escalation Rules
- 계획과 충돌하는 요구 발견 시 즉시 Planner로 반환

## Common Anti-Patterns
- 범위 외 리팩토링
- 테스트 생략 후 완료 선언
