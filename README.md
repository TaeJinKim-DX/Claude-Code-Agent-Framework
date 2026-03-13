# README.md

## 목적

이 문서는 사용자가 Claude Code를 이용해 이 저장소에서 개발 작업을 수행할 때 필요한 운영 가이드다.  
이 문서는 사람이 읽는 문서다.  
이 문서는 Claude Code가 어떤 문서를 읽고, 어떤 순서로 작업하며, 추가로 어떤 설정을 하면 더 안정적으로 동작하는지 안내한다.

이 문서의 목적은 다음과 같다.

- 사용자가 매번 긴 프롬프트를 작성하지 않아도 되게 한다
- Claude Code가 프로젝트 문서를 자동으로 더 잘 활용하도록 돕는다
- 파일 배치 위치를 명확히 하여 문서 충돌을 줄인다
- 개발 흐름을 분석 → 구현 → 검토 순서로 안정화한다

---

## 가장 중요한 결론

이 저장소에서 Claude Code를 안정적으로 쓰려면 아래만 기억하면 된다.

1. `CLAUDE.md`는 저장소 루트에 둔다
2. `AI_WORKFLOW.md`는 저장소 루트에 두고, `CLAUDE.md`에서 진입 문서로 연결한다
3. `plan.md`, `build.md`, `review.md`는 `.opencode/agents/`에 둔다
4. `runtime-python.md`, `runtime-typescript.md`는 `docs/ai/`에 둔다
5. 프로젝트 일반 소개 문서는 기존 `README.md`로 유지한다
6. 이 문서(`instruction.md`)는 사용자가 Claude Code에게 어떻게 일을 시킬지 참고하는 문서다
7. Claude Code 성능을 더 높이려면 `CLAUDE.md import`, `.claude/commands/`, `.claude/settings.json hooks`를 추가로 쓴다

---

## 권장 파일 배치

아래 구조를 권장한다.

```text
project/
├── README.md
├── instruction.md
├── CLAUDE.md
├── AI_WORKFLOW.md
├── .claude/
│   ├── settings.json
│   └── commands/
│       ├── plan.md
│       ├── implement.md
│       └── review.md
├── .opencode/
│   └── agents/
│       ├── plan.md
│       ├── build.md
│       └── review.md
└── docs/
    └── ai/
        ├── runtime-python.md
        └── runtime-typescript.md
```

---

## 각 파일의 역할

| 파일 | 위치 | 역할 |
|---|---|---|
| `README.md` | 루트 | 사람용 프로젝트 소개 문서 |
| `instruction.md` | 루트 | 사용자가 Claude Code를 어떻게 사용할지 설명하는 문서 |
| `CLAUDE.md` | 루트 | Claude Code가 자동으로 읽는 최상위 규칙 문서 |
| `AI_WORKFLOW.md` | 루트 | AI 작업 허브 문서 |
| `plan.md` | `.opencode/agents/` | 구현 전 분석 기준 |
| `build.md` | `.opencode/agents/` | 구현 규칙 |
| `review.md` | `.opencode/agents/` | 리뷰 및 품질 점검 기준 |
| `runtime-python.md` | `docs/ai/` | Python Agent 규칙 |
| `runtime-typescript.md` | `docs/ai/` | TypeScript Agent 규칙 |
| `.claude/commands/*.md` | `.claude/commands/` | Claude Code slash command 문서 |
| `.claude/settings.json` | `.claude/` | Claude Code 프로젝트 설정 및 hooks |

---

## Claude Code가 자동으로 읽는 것

Claude Code는 프로젝트 메모리로 `CLAUDE.md`를 자동 로드한다.  
또한 `CLAUDE.md` 안에서 `@path/to/file` 형식으로 다른 파일을 import할 수 있다.  
이 기능을 이용하면 루트 규칙 파일을 짧게 유지하면서도 필요한 세부 문서를 자동으로 연결할 수 있다. citeturn846654search3

즉, 가장 좋은 구조는 아래와 같다.

- `CLAUDE.md`는 짧고 강한 규칙만 둔다
- `AI_WORKFLOW.md`를 `CLAUDE.md`에서 import 또는 진입 문서로 연결한다
- 세부 문서는 `AI_WORKFLOW.md`에서 흐름상 참조한다

---

## 지금 구조에서 사용자가 해야 하는 기본 행동

### 작업 시작 시
- Claude Code에게 작업을 줄 때 긴 문서 목록을 매번 복붙하지 않는다
- 작업 내용만 명확히 준다
- 필요하면 제약 조건과 원하는 결과만 덧붙인다

### 기본 프롬프트 예시

아래 정도면 충분하다.

```text
아래 작업을 수행해.

작업:
- [여기에 실제 작업]

제약:
- 기존 인터페이스 유지
- LiteLLM Gateway 유지
- 테스트 함께 수정
- 불필요한 리팩토링 금지
```

`CLAUDE.md`와 `AI_WORKFLOW.md`가 제대로 연결되어 있으면 Claude Code가 나머지 문서를 따라가며 작업할 가능성이 높다.

---

## Claude Code 성능을 더 높이는 설정 3가지

아래 3가지는 지금 구조 위에 추가하면 효과가 크다.

### 1. `CLAUDE.md`에서 명시적으로 import 연결하기

`CLAUDE.md`는 자동 로드되므로, 여기에 필요한 문서를 직접 import하면 Claude Code가 더 적은 프롬프트로도 안정적으로 동작한다. Anthropic 공식 문서는 `CLAUDE.md`에서 `@README`, `@docs/...` 같은 import 구문을 지원한다고 설명한다. citeturn846654search3

권장 예시:

```md
# CLAUDE.md

See @AI_WORKFLOW.md for task execution flow.
See @.opencode/agents/plan.md for pre-implementation analysis.
See @.opencode/agents/build.md for implementation rules.
See @.opencode/agents/review.md for review rules.
See @docs/ai/runtime-python.md for Python tasks.
See @docs/ai/runtime-typescript.md for TypeScript tasks.
```

#### 왜 좋은가
- 프롬프트 없이도 진입 문서가 연결된다
- `CLAUDE.md`를 짧게 유지할 수 있다
- 문서 위치가 바뀌지 않는 한 반복 사용에 강하다

---

### 2. `.claude/commands/`에 slash command 만들기

Claude Code는 프로젝트별 slash command를 `.claude/commands/`에 markdown 파일로 둘 수 있다.  
예를 들어 `.claude/commands/plan.md`를 만들면 `/plan` 같은 방식으로 불러 쓸 수 있다. Anthropic 공식 문서는 프로젝트 명령 파일 위치를 `.claude/commands/`로 안내한다. citeturn846654search1

권장 예시:

#### `.claude/commands/plan.md`

```md
---
description: 작업 분석만 수행
argument-hint: [task]
---

Read CLAUDE.md and AI_WORKFLOW.md first.
Then perform only the planning phase for: $ARGUMENTS

Do not implement yet.
Focus on:
- requirements clarification
- impacted files
- interface impact
- test impact
- runtime choice
```

#### `.claude/commands/implement.md`

```md
---
description: 구현 진행
argument-hint: [task]
---

Read CLAUDE.md and AI_WORKFLOW.md first.
Use build.md and the proper runtime document.
Implement: $ARGUMENTS

Constraints:
- keep interface compatibility
- use LiteLLM Gateway
- update tests
```

#### `.claude/commands/review.md`

```md
---
description: 리뷰 및 검토
argument-hint: [scope]
---

Read CLAUDE.md and AI_WORKFLOW.md first.
Use review.md to validate: $ARGUMENTS

Check:
- interface compatibility
- LiteLLM compliance
- logging
- error handling
- tests
```

#### 왜 좋은가
- 사용자는 `/plan [작업]`, `/implement [작업]`, `/review [범위]`만 치면 된다
- 작업 단계를 강제할 수 있다
- 프롬프트 실수를 줄인다

---

### 3. `.claude/settings.json`에 hooks 넣기

Claude Code는 `.claude/settings.json`에서 hooks를 설정할 수 있다.  
공식 문서에 따르면 `PreToolUse`, `PostToolUse`, `UserPromptSubmit` 같은 이벤트에 명령을 붙일 수 있고, 특히 `UserPromptSubmit`의 stdout은 context에 들어간다. citeturn846654search2

이 기능을 이용하면 사용자가 프롬프트를 보낼 때마다 Claude Code에게 작업 흐름을 상기시키는 자동 문맥을 줄 수 있다.

권장 예시:

#### `.claude/settings.json`

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "printf 'Follow this workflow: CLAUDE.md -> AI_WORKFLOW.md -> plan.md -> runtime -> build.md -> review.md\nUse LiteLLM Gateway. Preserve interfaces. Update tests.'"
          }
        ]
      }
    ]
  }
}
```

#### 왜 좋은가
- 사용자가 짧게 말해도 기본 규칙을 매번 상기시킨다
- workflow 이탈 가능성을 줄인다
- 문서 다 읽으라는 프롬프트를 반복하지 않아도 된다

---

## 가장 추천하는 최종 구조

실전에서 가장 추천하는 조합은 아래다.

### 필수
- `CLAUDE.md`
- `AI_WORKFLOW.md`
- `.opencode/agents/plan.md`
- `.opencode/agents/build.md`
- `.opencode/agents/review.md`
- `docs/ai/runtime-python.md`
- `docs/ai/runtime-typescript.md`

### 있으면 더 좋은 것
- `.claude/commands/plan.md`
- `.claude/commands/implement.md`
- `.claude/commands/review.md`
- `.claude/settings.json`

---

## 사용자 입장에서 가장 쉬운 운영 방식

아래처럼 운영하면 된다.

### 1. 저장소 준비
- 문서를 위 위치에 둔다
- `CLAUDE.md`에서 `AI_WORKFLOW.md`를 연결한다
- 필요하면 `.claude/commands/`와 `.claude/settings.json`을 추가한다

### 2. Claude Code에 일 시키기
짧게 말한다.

예시:
- `RAG Agent에 문서 버전 필터 추가해`
- `TypeScript Agent LiteLLM alias 버그 수정해`
- `이 작업 plan만 먼저 해줘`

### 3. Claude Code가 따라야 할 흐름
- CLAUDE
- AI_WORKFLOW
- plan
- runtime
- build
- review

---

## 사용자가 해야 하는 최소 점검

- `CLAUDE.md`가 루트에 있는가
- `AI_WORKFLOW.md`가 루트에 있는가
- `plan.md`, `build.md`, `review.md`가 `.opencode/agents/`에 있는가
- `runtime-python.md`, `runtime-typescript.md`가 `docs/ai/`에 있는가
- `CLAUDE.md`가 `AI_WORKFLOW.md`를 진입 문서로 연결하고 있는가
- 필요하면 `.claude/commands/`와 `.claude/settings.json`을 추가했는가

---

## 금지사항

- 프로젝트 설명용 `README.md`에 AI 작업 규칙을 전부 섞어넣기
- `CLAUDE.md`를 너무 길고 설명형으로 만들기
- `AI_WORKFLOW.md`에서 다시 README를 필수 진입점으로 만들기
- runtime 규칙을 `CLAUDE.md`에 전부 중복 작성하기
- 사용자가 매번 긴 문서 목록을 프롬프트에 복붙하기

---

## 최종 결론

이 저장소에서 Claude Code를 잘 쓰려면 핵심은 다음이다.

- `CLAUDE.md`는 최상위 자동 규칙
- `AI_WORKFLOW.md`는 AI 작업 허브
- 세부 규칙은 `plan`, `build`, `review`, `runtime-*`로 분리
- 추가 최적화는 `CLAUDE.md import`, `.claude/commands/`, `.claude/settings.json hooks`

이 구조를 적용하면 사용자는 긴 프롬프트 없이도 짧은 작업 지시만으로 Claude Code를 안정적으로 운영할 수 있다.
