# /start-work Command Specification

## File Contract
- Why it exists: 작업 시작 시 컨텍스트 정렬과 범위 선언을 강제한다.
- What belongs: 시작 체크, 입력 계약, 초기 산출물 정의.
- What does not belong: 구현 상세 절차, 리뷰 판정 기준.
- Suggested section structure: Intent → Preconditions → Inputs → Protocol → Outputs → Validation → Failure → Stop.
- Typical mistakes: 시작 단계에서 구현을 먼저 시도함.
- Preventive guidance: `/start-work`는 탐색·정렬 전용으로 제한한다.

## Intent
작업 시작 전 컨텍스트·범위·검증 계획을 정렬한다.

## Preconditions / Preflight Checks
- `CLAUDE.md`, `COLLABORATION.md` 확인
- 기존 관련 계획/증거 검색

## Inputs
- Required: 작업 설명
- Optional: 관련 이슈/제약/우선순위

## Step-by-Step Execution Protocol
1. 작업 의도를 1문장으로 고정
2. 범위(In/Out) 임시 선언
3. 관련 규칙/명령/역할 문서 링크 수집
4. `/plan` 실행 준비 상태 보고

## Output Artifacts
- `.sisyphus/drafts/<task-id>-startup.md`

## Validation Checklist
- [ ] 작업 목적이 문장으로 명시됨
- [ ] 범위 초안 존재
- [ ] 참조 문서 목록 존재
- [ ] `.claude/harness/acceptance-checklist.md`의 시작 단계 항목 충족
- [ ] `.claude/harness/evidence-spec.md`의 evidence 최소 필드 이해/적용 준비 완료
- [ ] `.claude/harness/runbook.md`의 실패 triage 경로 확인

## Failure Modes and Retry Protocol
- 실패: 컨텍스트 불충분
- 재시도: 관련 파일 탐색 후 startup 초안 갱신

## Stop Conditions
- `/plan`으로 넘길 최소 컨텍스트가 준비되면 종료
