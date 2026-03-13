# AI_WORKFLOW.md

## 목적

이 문서는 이 저장소에서 AI 코딩 에이전트가 실제 개발 작업을 수행할 때 따르는 개발 실행 허브 문서다.

이 문서는 프로젝트 소개 문서가 아니다.  
이 문서는 개발 작업을 어떻게 진행해야 하는지 정의하는 실행 가이드다.  
이 문서는 AI 코딩 에이전트의 실제 작업 진입점이다.  
프로젝트 개요나 일반 사용 방법이 필요할 경우에만 `README.md`를 참고한다.

AI 코딩 에이전트는 이 문서를 기준으로 다음을 수행한다.

- 작업 분석
- 런타임 선택
- 코드 구현
- 테스트 수행
- 리뷰 및 검증
- 결과 요약

---

## 이 문서의 역할

이 문서는 아래 역할만 수행한다.

- AI 작업 진입점 제공
- 문서 참조 순서 정의
- 작업 흐름 정의
- Python / TypeScript 선택 기준 요약
- 작업 유형별 빠른 가이드 제공
- 세부 규칙 문서 연결

이 문서는 아래를 직접 대체하지 않는다.

- 프로젝트 소개 문서
- 상세 설계 분석 규칙
- 상세 구현 규칙
- 상세 리뷰 규칙
- Python 런타임 세부 규칙
- TypeScript 런타임 세부 규칙

세부 규칙은 별도 문서를 참조한다.

---

## 전체 작업 흐름

이 저장소에서 AI 코딩 에이전트는 아래 순서를 따른다.

1. `CLAUDE.md` 읽기
2. `AI_WORKFLOW.md` 읽기
3. 프로젝트 개요나 사용 방법이 필요하면 `README.md` 참고
4. `.opencode/agents/plan.md` 기준으로 분석
5. Python 또는 TypeScript 런타임 선택
6. 해당 runtime 문서 확인
7. `.opencode/agents/build.md` 기준으로 구현
8. `.opencode/agents/review.md` 기준으로 검토
9. 테스트 수행
10. 변경 내용 요약

기본 흐름:

CLAUDE.md  
→ AI_WORKFLOW.md  
→ 필요 시 README.md  
→ plan.md  
→ runtime 문서  
→ build.md  
→ review.md  

---

## 참조 문서

AI 코딩 에이전트는 아래 문서를 작업 중 참조해야 한다.

| 문서 | 역할 | 필수 여부 |
|---|---|---|
| `CLAUDE.md` | 최상위 개발 규칙 | 필수 |
| `AI_WORKFLOW.md` | AI 작업 허브 | 필수 |
| `README.md` | 프로젝트 개요 및 일반 사용법 | 필요 시 |
| `.opencode/agents/plan.md` | 구현 전 분석 | 필수 |
| `.opencode/agents/build.md` | 구현 규칙 | 필수 |
| `.opencode/agents/review.md` | 리뷰 및 검증 | 필수 |
| `docs/ai/runtime-python.md` | Python Agent 규칙 | Python 작업 시 |
| `docs/ai/runtime-typescript.md` | TypeScript Agent 규칙 | TypeScript 작업 시 |
| `instruction.md` | 사용자가 Claude Code를 사용하는 방법 | 사용자 참고용 |

---

## 런타임 선택 기준

| 상황 | 권장 런타임 |
|---|---|
| RAG / 검색 / 임베딩 | Python |
| 상태 기반 워크플로우 | Python |
| 데이터 처리 / AI 로직 | Python |
| 웹 UI 기반 Agent | TypeScript |
| streaming 응답 | TypeScript |
| Node.js 서비스 연계 | TypeScript |

추가 원칙:

- Python과 TypeScript는 경쟁 관계가 아니다
- 같은 기능을 두 런타임에 중복 구현하지 않는다
- 판단이 어려우면 먼저 `plan.md` 기준으로 분석한다

---

## LiteLLM 사용 규칙

모든 LLM 호출은 아래 구조를 따른다.

Application  
→ LiteLLM Gateway  
→ Model Provider  

필수 환경 변수:

- `LITELLM_BASE_URL`
- `LITELLM_VIRTUAL_KEY`
- `LITELLM_MODEL`

금지사항:

- Provider SDK 직접 호출
- API Key 하드코딩
- Gateway 우회 호출

---

## 공통 개발 원칙

모든 Agent는 아래 원칙을 따른다.

- LiteLLM Gateway를 우선 사용한다
- 공통 요청/응답 구조를 유지한다
- Tool 호출 전 입력 검증 및 정책 검증을 수행한다
- 구조화된 로그와 감사 이력을 남긴다
- 테스트 없는 핵심 변경은 하지 않는다
- Agent 토폴로지는 고정하지 않는다
- 기존 구조를 먼저 따른다
- side effect는 코드 구조상 명확히 드러나야 한다
- 설정값, 키, URL을 코드에 하드코딩하지 않는다
- 민감정보를 로그에 남기지 않는다

---

## 작업 유형별 실행 가이드

### 신규 기능 개발

1. `CLAUDE.md` 확인
2. `AI_WORKFLOW.md` 흐름 확인
3. `plan.md` 기준 분석
4. runtime 문서 선택
5. `build.md` 기준 구현
6. `review.md` 기준 검토

결과로 아래를 제공해야 한다.

- 설계 요약
- 변경 파일 목록
- 구현 요약
- 테스트 결과
- 남은 리스크

### 기존 기능 수정

확인할 사항:

- 인터페이스 영향 여부
- 호출자 영향 여부
- 테스트 영향 여부

작업 흐름:

plan  
→ runtime  
→ build  
→ review  

### 버그 수정

먼저 수행할 것:

- 버그 재현 조건 확인
- 영향 파일 파악
- 최소 수정 범위 설정

그 다음:

build  
→ review  
→ 테스트  

### Python Agent 작업

반드시 먼저 읽는다.

- `CLAUDE.md`
- `AI_WORKFLOW.md`
- `.opencode/agents/plan.md`
- `docs/ai/runtime-python.md`
- `.opencode/agents/build.md`
- `.opencode/agents/review.md`

핵심 기준:

- LangGraph 우선
- LangChain은 보조 사용
- Pydantic 계약 유지
- FastAPI는 API 레이어 역할만 수행
- httpx 사용
- LiteLLM Gateway 유지

### TypeScript Agent 작업

반드시 먼저 읽는다.

- `CLAUDE.md`
- `AI_WORKFLOW.md`
- `.opencode/agents/plan.md`
- `docs/ai/runtime-typescript.md`
- `.opencode/agents/build.md`
- `.opencode/agents/review.md`

핵심 기준:

- Node.js 기반
- LangGraph JS 우선
- LangChain.js는 보조 사용
- Zod 계약 유지
- LiteLLM Gateway 유지
- Next.js는 UI 계층일 때만 사용

---

## 작업 전 체크리스트

- 작업 목적을 한 문장으로 정의했는가
- 신규 기능인지, 수정인지, 버그인지 구분했는가
- 관련 파일과 영향 파일을 식별했는가
- 기존 인터페이스를 확인했는가
- 런타임 선택이 적절한가
- LiteLLM Gateway 경유 구조를 유지하는가
- 테스트 영향 범위를 확인했는가
- side effect 유무를 구분했는가

---

## 구현 중 체크리스트

- 기존 구조를 먼저 따르고 있는가
- 불필요한 새 레이어를 만들지 않았는가
- 공통 모듈 재사용 여부를 확인했는가
- 설정값을 하드코딩하지 않았는가
- 민감정보를 로그에 남기지 않았는가
- 테스트를 함께 수정하고 있는가
- 응답 구조를 임의로 바꾸지 않았는가

---

## 구현 후 체크리스트

- 정상 경로 테스트가 있는가
- 핵심 실패 경로를 검토했는가
- mock / offline 경로가 유지되는가
- LiteLLM 우회 구현이 숨어 있지 않은가
- 인터페이스 호환성이 유지되는가
- 로깅 / 에러 처리 / 감사 이력이 유지되는가
- 필요한 문서 또는 샘플 설정을 갱신했는가

---

## 금지사항 요약

아래는 공통 금지사항이다.

- 요구사항 파악 없이 바로 구현 시작
- LiteLLM Gateway를 이유 없이 우회
- 테스트 없이 핵심 로직 수정
- 민감정보를 코드나 로그에 직접 기록
- 공통 인터페이스를 조용히 변경
- 같은 역할의 로직을 여러 언어/파일에 중복 구현
- UI 계층에 Agent 핵심 로직 직접 구현
- side effect를 암묵적으로 숨김
- 목적과 무관한 리팩토링 동시 수행

상세 금지사항은 각 문서를 따로 본다.

---

## Claude Code에게 일을 시킬 때의 기본 원칙

사용자는 Claude Code에게 작업을 시킬 때 아래 원칙을 따른다.

- 이 문서를 먼저 읽게 한다
- 작업 내용을 명확하게 쓴다
- 필요한 제약 조건을 같이 준다
- 가능하면 분석 → 구현 → 검토 흐름으로 시킨다
- 원하는 결과 형식을 같이 준다

최소한 아래 4가지를 포함한다.

- 작업 내용
- 제약 조건
- 참조 문서
- 원하는 결과

---

## 추천 저장소 구조 예시

```text
.
├── README.md
├── AI_WORKFLOW.md
├── CLAUDE.md
├── instruction.md
├── .opencode/
│   └── agents/
│       ├── plan.md
│       ├── build.md
│       └── review.md
├── docs/
│   └── ai/
│       ├── runtime-python.md
│       └── runtime-typescript.md
├── agent/
├── tools/
├── api/
├── service/
├── tests/
└── deploy/
```

이 구조는 예시다.  
실제 디렉터리 구조는 저장소 상황에 맞게 조정할 수 있다.  
단, 문서 위치와 역할은 일관되게 유지한다.

---

## 이 문서가 다루지 않는 범위

이 문서는 허브 문서다.  
이 문서는 세부 구현 규칙을 모두 담지 않는다.

아래 내용은 반드시 개별 문서를 본다.

- 설계 및 영향도 분석 상세 규칙 → `plan.md`
- 구현 행동 규칙 → `build.md`
- 리뷰 및 품질 점검 기준 → `review.md`
- Python 런타임 상세 규칙 → `runtime-python.md`
- TypeScript 런타임 상세 규칙 → `runtime-typescript.md`

---

## 최종 행동 규칙

이 문서를 읽은 후에는 아래를 바로 실행한다.

- 공통 규칙은 `CLAUDE.md`에서 확인한다
- 구현 전 분석은 `plan.md`에서 수행한다
- 런타임은 목적 기준으로 선택한다
- 구현은 `build.md`를 기준으로 진행한다
- 검토는 `review.md`를 기준으로 수행한다
- Python 작업은 `runtime-python.md`
- TypeScript 작업은 `runtime-typescript.md`

이 문서는 시작점이다.  
세부 규칙은 역할에 맞는 문서를 즉시 이어서 읽고 작업한다.
