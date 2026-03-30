# scope-control.md

## Why it exists
요구된 작업만 수행하도록 범위를 통제한다.

## Policy
- In Scope 외 변경 금지
- “같이 고치면 좋아 보이는” 변경 금지
- 범위 변경 필요 시 decision record 생성 후 승인

## Change Boundary Rules
- 파일 추가/삭제는 계획서에 사전 명시
- 공통 규칙 파일 변경은 리뷰 필수

## Typical mistakes
- 리팩토링을 기능 작업에 묶어서 진행

## Preventive guidance
- 구현 중 새 요구가 생기면 현재 작업을 끝내고 별도 task-id로 분리한다.
