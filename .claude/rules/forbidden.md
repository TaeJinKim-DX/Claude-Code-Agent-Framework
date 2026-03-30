# forbidden.md

## Why it exists
프로젝트 안정성을 해치는 행동을 하드 금지로 명시한다.

## Hard Constraints
- 승인 없는 범위 확장 금지
- 증거 없는 완료 선언 금지
- 역할 경계 위반 금지
- 민감정보 하드코딩 금지
- 테스트/검증 생략 후 머지 금지

## Typical mistakes
- “작동할 것 같음” 근거로 완료 처리

## Preventive guidance
- 완료 전 `quality-gates.md` 체크를 강제한다.
