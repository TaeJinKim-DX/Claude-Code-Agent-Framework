# Claude Agent Skeleton Standard

이 저장소는 특정 도메인 전용이 아니라, 어떤 팀/프로젝트에도 재사용 가능한 **Claude Code 운영 스켈레톤**입니다.

핵심은 `.claude` 중심 운영, 3역할(Planner/Builder/Reviewer), 5명령(`/start-work`, `/plan`, `/implement`, `/review`, `/handoff`), 그리고 `.sisyphus/evidence` 기반 증거 관리입니다.

## 빠른 시작

1. `CLAUDE.md` 읽기
2. `COLLABORATION.md` 읽기
3. `.claude/commands/start-work.md` 실행 계약 확인
4. 첫 작업에서 `/plan → /implement → /review` 순서 적용

## 왜 `.claude` 구조를 쓰는가 (간략)

- 정책(`rules`), 실행계약(`commands`), 역할(`agents`)이 분리되어 책임이 섞이지 않습니다.
- 하네스(`harness`)와 템플릿(`templates`)을 통해 작업 품질을 반복 가능하게 유지합니다.
- 팀/프로젝트가 바뀌어도 파일 위치와 계약이 같아 온보딩이 빨라집니다.

## 프로젝트 커스터마이징 가이드

### 1) 역할/명령 이름 커스터마이징
- 기본 역할: Planner/Builder/Reviewer
- 기본 명령: `/start-work`, `/plan`, `/implement`, `/review`, `/handoff`
- 바꿀 때는 반드시 아래를 **동시에** 수정
  - `.claude/agents/*.md`
  - `.claude/commands/*.md`
  - `.claude/rules/quality-gates.md`
  - `.claude/harness/acceptance-checklist.md`

### 2) 도메인 용어 반영
- 금지: 특정 프로젝트 하드코딩 용어(예: 팀 내부 코드명, 특정 서비스명)
- 권장: `domain module`, `service`, `adapter`, `contract` 같은 일반 용어 사용
- 도메인 특수 용어는 `.claude/glossary/terms.md`에만 등록

### 3) 소유권/브랜치 전략 맞춤화
- 소유권 정책: `.claude/rules/ownership.md`
- 범위 통제 정책: `.claude/rules/scope-control.md`
- 팀 구조(1인/다인)에 맞게 “승인 필요 경로”와 “공동 소유 경로”만 수정

### 4) 하네스 강도 조정
- 최소 기준: 산출물 존재/필수 섹션/리뷰 verdict/evidence 링크
- 강화 기준(선택): 테스트 커버리지, 보안 스캔, 배포 전 smoke test
- 변경 위치: `.claude/harness/*` + `.claude/rules/quality-gates.md`

### 5) 산출물 경로 맞춤화
- 기본 경로: `.sisyphus/plans`, `.sisyphus/drafts`, `.sisyphus/evidence`
- 변경 시 모든 명령/하네스 문서에서 동일 경로로 동기화 필요

### 6) 이름 변경 동기화 예시

| 변경 항목 | 수정 파일 |
|---|---|
| 역할명 `Planner` → `Designer` | `.claude/agents/role001-planner.md`, `.claude/commands/plan.md`, `.claude/rules/quality-gates.md` |
| 명령 `/implement` → `/build` | `.claude/commands/implement.md`, `CLAUDE.md`, `COLLABORATION.md`, `.claude/settings.json` |
| 증거 경로 변경 | `.claude/settings.json`, `.claude/harness/*`, `.claude/commands/*`, `COLLABORATION.md` |

### 7) 커스터마이징 완료 검증
- 역할/명령 변경 후 용어 충돌 검색(`grep`) 결과 0건 확인
- `COLLABORATION.md`의 11개 필수 섹션 유지 확인
- `.claude/harness/acceptance-checklist.md`와 `.claude/rules/quality-gates.md` 동기화 확인
- `.sisyphus` 경로 존재 및 산출물 기록 가능 여부 확인

### 8) 산출물은 `docs/`에서 관리
- 권장: 팀 공유용 최종 산출물은 `docs/` 하위에 저장
  - 예: `docs/decisions/`, `docs/specs/`, `docs/reports/`, `docs/releases/`
- 운영 증거/작업 중간물은 기존대로 `.sisyphus/`에 유지
  - `.sisyphus/plans`, `.sisyphus/drafts`, `.sisyphus/evidence`

### 9) Claude 요청 예시 (docs 산출물 기준)
```text
다음 작업을 수행해줘.
- 결과 산출물은 docs/reports/2026-xx-xx-<topic>.md 로 생성
- 의사결정 내용은 docs/decisions/ADR-xxxx.md 로 기록
- 작업 중간 증거는 .sisyphus/evidence/<task-id>/ 에 남겨
- /plan -> /implement -> /review 순서로 진행
```

## 운영 원칙

- 단일 정책 소스: `COLLABORATION.md`
- 실행 계약 소스: `.claude/commands/*`
- 역할 경계 소스: `.claude/agents/*`
- 품질 게이트 소스: `.claude/rules/quality-gates.md`
- 런타임 가이드 소스: `.claude/runtime/*`

## 주의사항

- 레거시 운영 문서나 특정 프로젝트 고유 규칙을 기본값으로 두지 마세요.
- 커스터마이징은 “이름만” 바꾸지 말고, 규칙/게이트/하네스까지 연동해 일관성 있게 반영해야 합니다.
