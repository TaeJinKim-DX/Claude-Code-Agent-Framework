# CLAUDE.md

## 목적

이 문서는 이 저장소의 최상위 운영 정책이다. 이 저장소는 **Claude Code 전용, `.claude` 중심**으로 운영한다.

## 단일 운영 모델

- 역할: Planner / Builder / Reviewer
- 명령: `/plan`, `/implement`, `/review`, `/start-work`, `/handoff`
- 거버넌스 앵커: `CLAUDE.md`, `COLLABORATION.md`
- 증거 저장 경로: `.sisyphus/evidence/`

## 실행 순서 (고정)

1. `CLAUDE.md` 확인
2. `COLLABORATION.md` 확인
3. `.claude/commands/start-work.md` 기준으로 작업 시작
4. `/plan` → `/implement` → `/review` 순서 수행
5. 필요 시 `/handoff`로 인수인계 문서화

## 강제 규칙

- 정책의 단일 소스는 `COLLABORATION.md`다.
- 명령 계약, 역할 경계, 품질 게이트를 우회하지 않는다.
- 승인되지 않은 범위 확장(스코프 크립)을 금지한다.
- 모든 완료 선언은 증거(artifact + checklist + review 결과)로 뒷받침한다.

## 금지

- 레거시 운영 문서 의존
- Planner/Builder/Reviewer 역할 혼합 수행
- 테스트/검증 없이 완료 처리

## 문서 인덱스

- 운영 계약: `COLLABORATION.md`
- 역할: `.claude/agents/`
- 명령: `.claude/commands/`
- 규칙: `.claude/rules/`
- 템플릿: `.claude/templates/`
- 하네스: `.claude/harness/`
- 워크플로: `.claude/workflows/`
- 런타임 가이드: `.claude/runtime/`
- 용어: `.claude/glossary/terms.md`

## 변경 원칙

- 이 문서는 최소·안정 정책만 담는다.
- 세부 규칙 변경은 `COLLABORATION.md`와 하위 `.claude/*` 문서에서 수행한다.
