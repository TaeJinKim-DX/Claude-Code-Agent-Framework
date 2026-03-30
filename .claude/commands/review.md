# /review Command Specification

## File Contract
- Why it exists: 품질 게이트 기반 승인/반려 판단을 일관되게 수행하기 위해 존재한다.
- What belongs: 게이트 평가, 결함 기록, verdict.
- What does not belong: 대규모 재구현.
- Suggested section structure: Intent → Preconditions → Inputs → Protocol → Outputs → Validation → Failure → Stop.
- Typical mistakes: 스타일 리뷰만 수행함.
- Preventive guidance: scope/quality/evidence gate를 모두 평가한다.

## Intent
계획·구현·증거의 정합성을 평가해 승인 여부를 판정한다.

## Preconditions / Preflight Checks
- 계획서/구현로그/검증결과 존재

## Inputs
- Required: 리뷰 대상 범위(파일 또는 task-id)
- Optional: 우선 검토 리스크

## Step-by-Step Execution Protocol
1. 계획 대비 변경사항 대조
2. 품질 게이트 체크
3. 결함/리스크 정리
4. Verdict 작성

## Output Artifacts
- `.sisyphus/evidence/<task-id>/review-report.md`

## Validation Checklist
- [ ] scope gate 평가 완료
- [ ] quality gate 평가 완료
- [ ] verdict 명시
- [ ] `.claude/harness/acceptance-checklist.md`와 `.claude/harness/evidence-spec.md`를 근거로 판정
- [ ] 실패 처리/재시도 판단에 `.claude/harness/runbook.md` 기준 적용

## Failure Modes and Retry Protocol
- 실패: 증거 부족 또는 재현 불가
- 재시도: Builder에 보완 항목 전달 후 재검토

## Stop Conditions
- Verdict가 APPROVED 또는 BLOCKED로 확정되면 종료
