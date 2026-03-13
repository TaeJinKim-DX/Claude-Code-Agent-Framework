# instruction.md

## 목적

이 문서는 사용자가 Claude Code를 이용해 이 저장소에서 개발 작업을 수행할 때 참고하는 사용자용 가이드다.

이 문서는 아래 내용을 설명한다.

- 각 md 파일이 무엇을 하는지
- 각 md 파일을 어디에 둬야 하는지
- Claude Code에게 어떤 방식으로 일을 시키면 되는지
- 작업을 어떤 순서로 진행하면 되는지

이 문서는 사람이 읽는 문서다.  
Claude Code 자동 설정 문서는 아니다.

---

## 이 문서가 다루는 것

이 문서는 아래 내용을 다룬다.

- 파일 배치 위치
- 파일별 역할
- Claude Code 사용 방법
- 작업 지시 예시
- 추천 작업 흐름

---

## 이 문서가 다루지 않는 것

이 문서는 아래 내용을 직접 다루지 않는다.

- Claude Code 내부 설정 최적화
- hooks 설정
- slash command 설정
- CLAUDE.md 내부 규칙 상세 내용
- build / review / runtime 문서의 세부 규칙

이런 내용은 해당 문서 또는 설정 파일에서 관리한다.

---

## 권장 파일 배치

아래 구조를 권장한다.

```text
project/
├── README.md
├── instruction.md
├── CLAUDE.md
├── AI_WORKFLOW.md
│
├── .claude/
│   ├── settings.json
│   └── commands/
│       ├── plan.md
│       ├── implement.md
│       └── review.md
│
├── .opencode/
│   └── agents/
│       ├── plan.md
│       ├── build.md
│       └── review.md
│
└── docs/
    └── ai/
        ├── runtime-python.md
        └── runtime-typescript.md
```

---

## 파일별 역할

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

---

## 기본 원칙

이 저장소에서 Claude Code를 사용할 때는 아래 원칙을 따른다.

- 작업은 항상 짧고 명확하게 지시한다
- Claude Code가 `CLAUDE.md`를 자동으로 읽는다는 점을 전제로 한다
- AI 작업 흐름은 `AI_WORKFLOW.md`를 기준으로 한다
- 구현 전에는 분석을 먼저 시킨다
- 구현 후에는 검토를 하게 한다
- Python과 TypeScript는 역할에 따라 선택한다
- LiteLLM Gateway 원칙은 항상 유지한다

---

## Claude Code가 참고하는 흐름

Claude Code는 일반적으로 아래 순서로 문서를 참고한다.

`CLAUDE.md`  
→ `AI_WORKFLOW.md`  
→ `plan.md`  
→ `runtime-python.md` 또는 `runtime-typescript.md`  
→ `build.md`  
→ `review.md`

프로젝트 개요나 일반 사용법이 필요할 때만 `README.md`를 참고한다.

---

## 사용자가 Claude Code에게 일을 시키는 방법

사용자는 Claude Code에게 아래 방식으로 작업을 지시하면 된다.

### 가장 기본적인 방식

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

이 정도면 기본 작업에는 충분하다.

---

## 가장 추천하는 작업 방식

가장 추천하는 방식은 분석 → 구현 → 검토 순서다.

### 1단계: 분석 먼저

```text
이 작업을 먼저 분석만 해줘.

작업:
- [작업 내용]

원하는 결과:
- 요구사항 해석
- 영향 파일
- 인터페이스 영향 여부
- 테스트 영향 범위
- 구현 계획
```

### 2단계: 구현

```text
방금 분석한 내용을 기준으로 구현해.

조건:
- 기존 인터페이스 유지
- LiteLLM Gateway 유지
- 테스트 함께 수정
- 불필요한 리팩토링 금지
```

### 3단계: 검토

```text
이제 review 단계로 검토해.

확인할 것:
- 인터페이스 호환성
- LiteLLM 경유 유지
- 테스트 누락 여부
- 로깅 / 에러 처리
- 남은 리스크
```

---

## 작업 유형별 예시

### 신규 기능 추가

```text
이 작업을 분석 → 구현 → 검토 순서로 진행해.

작업:
- [새 기능 설명]

조건:
- 기존 구조 우선 사용
- LiteLLM Gateway 유지
- 테스트 추가
- 인터페이스 변경 최소화
```

### 기존 기능 수정

```text
아래 수정 작업을 진행해.

작업:
- [수정할 기능 설명]

조건:
- 최소 수정 원칙 적용
- 기존 인터페이스 유지
- 테스트 수정
```

### 버그 수정

```text
아래 버그를 수정해.

버그:
- [버그 설명]

조건:
- 재현 조건 먼저 정리
- 최소 수정 원칙 적용
- 기존 인터페이스 유지
- 테스트 추가 또는 수정
```

### Python Agent 작업

```text
아래 Python Agent 작업을 진행해.

작업:
- [Python 작업 내용]

조건:
- LangGraph 우선
- LangChain은 보조 사용
- Pydantic 계약 유지
- FastAPI는 API 레이어 역할만 수행
- LiteLLM Gateway 유지
```

### TypeScript Agent 작업

```text
아래 TypeScript Agent 작업을 진행해.

작업:
- [TypeScript 작업 내용]

조건:
- Node.js 기반 Agent 구조 유지
- LangGraph JS 우선
- LangChain.js는 보조 사용
- Zod 계약 유지
- LiteLLM Gateway 유지
- Next.js는 UI 계층일 때만 사용
```

---

## 사용자 체크리스트

Claude Code에게 작업을 시키기 전에 아래를 확인한다.

- 작업 목적을 한 문장으로 설명할 수 있는가
- 신규 기능인지, 수정인지, 버그인지 구분했는가
- Python인지 TypeScript인지 대략 판단했는가
- 인터페이스를 깨면 안 되는지 알고 있는가
- 테스트를 같이 수정해야 하는지 알고 있는가
- 원하는 결과를 정리했는가

---

## 금지사항

사용자가 Claude Code에게 작업을 시킬 때 아래 방식은 피한다.

- `코드 좀 고쳐줘`
- `알아서 해줘`
- `이거 개선해줘`
- `대충 구현해줘`

이런 지시는 범위가 모호해서 결과 품질이 불안정해진다.

항상 아래를 포함하는 것이 좋다.

- 작업 내용
- 제약 조건
- 원하는 결과

---

## 한 줄 정리

이 저장소에서 Claude Code를 사용할 때는 아래만 기억하면 된다.

- 파일은 정해진 위치에 둔다
- Claude Code는 `CLAUDE.md`와 `AI_WORKFLOW.md`를 기준으로 움직인다
- 작업은 분석 → 구현 → 검토 순서로 시킨다
- 사용자는 작업 내용, 제약 조건, 원하는 결과만 명확히 주면 된다
