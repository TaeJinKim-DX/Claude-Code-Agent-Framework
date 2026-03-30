# Agents Directory Contract

## Why this exists
- 역할(Planner/Builder/Reviewer)의 책임·경계·입출력 계약을 고정한다.

## What belongs here
- 역할 명세서, 인수인계 계약서.

## What does NOT belong here
- 구현 코드, 제품 요구사항 문서, 실험 메모.

## Suggested structure
- `role001-planner.md`
- `role002-builder.md`
- `role003-reviewer.md`
- `handoff-contract.md`

## Typical mistakes
- 역할 경계를 모호하게 작성
- 한 역할 문서에 다른 역할 책임을 섞음

## Preventive guidance
- 각 문서에 Mission/Out-of-Scope/Input/Output/Handoff를 고정 섹션으로 유지한다.
