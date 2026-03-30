# lifecycle.md

## Standard Lifecycle
`/start-work` → `/plan` → `/implement` → `/review` → `/handoff`

## Rollback/Recovery Path
- 리뷰 실패 시 `/implement`로 회귀
- 범위 충돌 시 `/plan`으로 회귀

## Exit Criteria
- review verdict = APPROVED
- evidence 링크 확인 완료
