# ownership.md

## Why it exists
다인 협업 시 충돌과 무단 변경을 줄이기 위해 수정 권한 경계를 정의한다.

## What belongs
- 공동 소유 경로
- 승인 필요 경로
- 단독 수정 가능 경로

## What does not belong
- 기능 구현 설명

## Ownership Policy
- 공동 소유(리뷰 1인 이상 필수):
  - `CLAUDE.md`
  - `COLLABORATION.md`
  - `.claude/rules/*`
  - `.claude/harness/*`
- 단독 수정 가능(작업 범위 내):
  - 기능별 도메인 문서/코드
- 승인 필요:
  - 공통 계약, 명령 규약, 품질게이트 변경

## Typical mistakes
- 공동 소유 파일을 단독 변경

## Preventive guidance
- 변경 전 ownership 표를 확인하고 필요한 리뷰어를 지정한다.
