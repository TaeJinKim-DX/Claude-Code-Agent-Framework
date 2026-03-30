# Acceptance Checklist

## Artifact Existence Checks
- [ ] 계획서 존재 (`.sisyphus/plans/...`)
- [ ] 구현 로그 존재 (`.sisyphus/drafts/...`)
- [ ] 리뷰 보고서 존재 (`.sisyphus/evidence/...`)

## Artifact Quality Sanity Checks
- [ ] 필수 섹션 누락 없음
- [ ] 본문 비어있지 않음
- [ ] task-id 일관성 유지

## Command Output Checks
- [ ] `/plan` 산출물 형식 준수
- [ ] `/implement` 검증 결과 포함
- [ ] `/review` verdict 명시

## Review Gate Policy
- [ ] BLOCKED 상태 해소 없이 종료 금지
- [ ] CHANGES_REQUIRED는 수정 후 재리뷰 필수
