# COLLABORATION.md

## 1. Executive Summary

이 저장소는 Claude Code가 장기적으로 안정 운영할 수 있도록 표준화된 협업/실행 스켈레톤을 제공한다. 핵심은 `.claude` 중심의 명령 기반 워크플로, 3역할 분리(Planner/Builder/Reviewer), 증거 중심 품질 게이트다.

본 표준은 도메인 로직이 아닌 운영 체계에 집중하며, 신규 기여자는 30분 이내에 작업 흐름을 이해하고 재현 가능해야 한다.

## 2. Recommended Repository Tree

```text
.
├── CLAUDE.md
├── COLLABORATION.md
├── .claude/
│   ├── settings.json
│   ├── agents/
│   │   ├── README.md
│   │   ├── role001-planner.md
│   │   ├── role002-builder.md
│   │   ├── role003-reviewer.md
│   │   └── handoff-contract.md
│   ├── commands/
│   │   ├── README.md
│   │   ├── plan.md
│   │   ├── implement.md
│   │   ├── review.md
│   │   ├── start-work.md
│   │   └── handoff.md
│   ├── rules/
│   │   ├── README.md
│   │   ├── ownership.md
│   │   ├── forbidden.md
│   │   ├── quality-gates.md
│   │   └── scope-control.md
│   ├── templates/
│   │   ├── task-plan-template.md
│   │   ├── implementation-log-template.md
│   │   ├── review-report-template.md
│   │   └── decision-record-template.md
│   ├── harness/
│   │   ├── README.md
│   │   ├── acceptance-checklist.md
│   │   ├── evidence-spec.md
│   │   └── runbook.md
│   ├── workflows/
│   │   ├── lifecycle.md
│   │   ├── escalation.md
│   │   └── incident-recovery.md
│   ├── runtime/
│   │   ├── runtime-python.md
│   │   └── runtime-typescript.md
│   └── glossary/
│       └── terms.md
└── .sisyphus/
    ├── plans/
    ├── drafts/
    └── evidence/
```

## 3. Folder-by-Folder Purpose

- `.claude/agents`: 역할 정의와 인수인계 계약의 단일 저장소.
- `.claude/commands`: 5개 표준 명령의 deterministic 실행 계약.
- `.claude/rules`: 소유권/금지/품질게이트/스코프 통제 정책.
- `.claude/templates`: 계획·구현·리뷰·의사결정 기록 산출물 템플릿.
- `.claude/harness`: 산출물 존재/품질/리뷰게이트/증거 규칙을 검증하는 운영 하네스.
- `.claude/workflows`: 수명주기, 에스컬레이션, 장애 복구 플로우.
- `.claude/runtime`: 런타임별 운영 가이드(Python/TypeScript).
- `.claude/glossary`: 용어 통일.
- `.sisyphus/plans|drafts|evidence`: 작업 중간산출물·최종증거 저장소.

## 4. Key File Templates (full content)

아래 템플릿 전문은 운영 기준 예시이며, 실제 적용 시 최종 원본은 `.claude/templates/*.md`를 기준으로 동기화한다.

### 4.1 task-plan-template.md

```md
# Task Plan

## Metadata
- task_id:
- owner_role: Planner
- created_at:
- related_issue:

## Objective

## In Scope / Out of Scope

## Constraints

## Assumptions

## Execution Plan
1.
2.
3.

## Risks and Mitigations

## Acceptance Criteria (binary)
- [ ] AC-01
- [ ] AC-02

## Evidence Plan
- expected_artifacts:
- expected_checks:
```

### 4.2 implementation-log-template.md

```md
# Implementation Log

## Metadata
- task_id:
- owner_role: Builder
- started_at:
- completed_at:

## Inputs
- plan_path:
- dependencies:

## Change Summary

## Files Changed
- path:
- reason:

## Verification
- diagnostics:
- tests:
- manual_qa:

## Deviations from Plan

## Remaining Risks

## Evidence Links
-
```

### 4.3 review-report-template.md

```md
# Review Report

## Metadata
- task_id:
- owner_role: Reviewer
- reviewed_at:

## Inputs Checked
- plan:
- implementation_log:
- changed_files:

## Gate Results
- scope_gate: PASS/FAIL
- quality_gate: PASS/FAIL
- evidence_gate: PASS/FAIL

## Findings
- severity:
- description:
- required_action:

## Verdict
- APPROVED / CHANGES_REQUIRED / BLOCKED

## Follow-up
```

### 4.4 decision-record-template.md

```md
# Decision Record

## Metadata
- decision_id:
- date:
- owner:

## Context

## Decision

## Alternatives Considered
1.
2.

## Consequences
- positive:
- negative:

## Rollback Plan

## Linked Evidence
```

## 5. Command Specifications (all 5 commands)

명령 전문은 `.claude/commands/*.md`를 따른다. 공통 계약은 아래와 같다.

- Intent: 명령의 목적과 역할 경계
- Preconditions / Preflight: 필수 입력, 선행 산출물, 차단 조건
- Inputs: required/optional
- Execution Protocol: 단계별 실행 규약
- Output Artifacts: 생성 산출물 경로
- Validation Checklist: pass/fail 항목
- Failure & Retry: 실패 분류와 재시도 규칙
- Stop Conditions: 종료 조건

## 6. Agent Role Specifications (all 3 roles)

역할 전문은 `.claude/agents/*.md`를 따른다. 공통 계약은 아래와 같다.

- Mission
- Responsibilities
- Out-of-Scope
- Input Contract
- Output Contract
- Handoff Requirements
- Escalation Rules
- Anti-Patterns

필수 인수인계 스키마는 `.claude/agents/handoff-contract.md`를 따른다.

## 7. Rule System

정책 단일 소스는 `.claude/rules`다.

- `ownership.md`: 수정 권한과 승인 경계
- `forbidden.md`: 하드 금지 사항
- `quality-gates.md`: Plan/Build/Review DoD 및 pass/fail
- `scope-control.md`: 범위 통제와 변경 경계

Definition of Done (요약):

1. Planner: 승인 가능한 계획 + 이진 수용기준 + 증거 계획
2. Builder: 계획 준수 구현 + 검증 결과 + 증거 첨부
3. Reviewer: 게이트 통과 판정 + 리스크/후속조치 명시

## 8. Harness Specification

하네스 단일 소스는 `.claude/harness`다.

- 산출물 존재 검사
- 산출물 품질 sanity 검사(필수 섹션/비어있지 않음)
- 명령 출력 계약 검사
- 리뷰 게이트 정책 검사
- 증거 캡처 규약 검사(`.sisyphus/evidence/...`)
- 실패 triage 및 책임자 지정

## 9. Migration Plan (legacy assets -> standardized `.claude`)

| Legacy Path | Target Path | Action | Risk Notes | Verification Step |
|---|---|---|---|---|
| `<legacy_process>/plan.*` | `.claude/commands/plan.md` + `.claude/agents/role001-planner.md` | split | 분석 규칙 누락 위험 | plan command와 planner role에 필수 섹션 존재 확인 |
| `<legacy_process>/build.*` | `.claude/commands/implement.md` + `.claude/agents/role002-builder.md` | split | 구현 규칙 분실 위험 | implement command 체크리스트/실패 모드 확인 |
| `<legacy_process>/review.*` | `.claude/commands/review.md` + `.claude/agents/role003-reviewer.md` | split | 리뷰 기준 모호화 위험 | quality gate 연계 확인 |
| `.claude/commands/{plan,implement,review}.md`(기존) | 동일 경로 | replace | 기존 사용자 습관 충돌 | 새 명령 스펙 8섹션 완비 확인 |
| `<legacy_root>/*` 기타 | 유지(레거시) 또는 단계적 archive | archive(권장) | 갑작스런 삭제 시 회귀 위험 | 신규 운영에서 미참조 확인 후 archive |

레거시가 부분 누락된 경우 fallback:

- 존재하는 문서만 매핑하고, 누락 항목은 `.claude/templates` 기반으로 재생성한다.
- 누락 사실과 보완 근거를 decision record로 남긴다.

## 10. Maintenance Playbook

월 1회 유지보수 루틴:

1. 명령 문서와 역할 문서의 계약 불일치 검사
2. rules/quality-gates와 harness 체크리스트 동기화 검사
3. 최근 5개 작업의 evidence 완결성 샘플링 검사
4. glossary 용어 드리프트 정리
5. 정책 변경 시 decision record 생성

운영 원칙:

- 정책은 추가보다 축소/명확화 우선
- 중복 규칙 금지(참조 링크로 연결)
- 파괴적 변경은 한 번에 1축만 적용

## 11. Operational Lifecycle (steady state)

초기 세팅 이후에는 아래 고정 루프만 유지한다.

1. `/start-work`
2. `/plan`
3. `/implement`
4. `/review`
5. 필요 시 `/handoff`

완료 기준:

- plan/build/review 산출물 존재
- evidence 링크 유효
- review verdict가 APPROVED
