# README.md

## 프로젝트 개요

이 저장소는 AI Agent 플랫폼 및 서비스 개발을 위한 저장소다.  
Python 기반 Agent와 TypeScript 기반 Agent를 모두 수용한다.  
모든 Agent는 공통 인터페이스, 공통 정책, 공통 테스트 원칙을 최대한 공유한다.  
LLM 호출은 LiteLLM Gateway를 공통 경로로 사용한다.

이 README는 소개 문서가 아니라 **개발 실행 허브 문서**다.  
이 저장소에서 작업을 시작하는 AI 코딩 에이전트와 개발자는 이 문서를 먼저 읽고, 이후 필요한 세부 규칙 문서를 순서대로 참조한다.

---

## 이 README의 역할

이 README는 아래 역할만 수행한다.

- 프로젝트 개발 진입점 제공
- 문서 참조 순서 정의
- 작업 흐름 정의
- Python / TypeScript 선택 기준 요약
- 작업 유형별 빠른 가이드 제공
- 세부 문서의 역할 연결

이 README는 아래를 직접 대체하지 않는다.

- 상세 설계 분석 규칙
- 상세 구현 규칙
- 상세 리뷰 규칙
- Python 런타임 세부 규칙
- TypeScript 런타임 세부 규칙

세부 규칙은 별도 문서를 본다.

---

## 가장 먼저 읽을 문서

모든 작업은 아래 순서를 기본으로 따른다.

1. `README.md`
2. `CLAUDE.md`
3. `.opencode/agents/plan.md`
4. 런타임 문서
   - Python 작업: `docs/ai/runtime-python.md`
   - TypeScript 작업: `docs/ai/runtime-typescript.md`
5. `.opencode/agents/build.md`
6. `.opencode/agents/review.md`

기본 흐름:

README.md  
→ CLAUDE.md  
→ plan.md  
→ runtime 문서  
→ build.md  
→ review.md  

---

## 문서 역할 요약

| 문서 | 역할 | 언제 읽는가 |
|---|---|---|
| `README.md` | 프로젝트 개요, 작업 흐름, 문서 허브 | 모든 작업 시작 전 |
| `CLAUDE.md` | 최상위 공통 규칙, 자동 workflow, 공통 금지사항 | 모든 작업 시작 전 |
| `.opencode/agents/plan.md` | 구현 전 분석, 영향도 파악, 리스크 점검 | 구현 전 |
| `.opencode/agents/build.md` | 실제 코드 작성 및 수정 규칙 | 구현 중 |
| `.opencode/agents/review.md` | 리뷰, 품질 점검, 인터페이스/테스트 검토 | 구현 후 |
| `docs/ai/runtime-python.md` | Python Agent 런타임 및 구현 규칙 | Python 작업 시 |
| `docs/ai/runtime-typescript.md` | TypeScript Agent 런타임 및 구현 규칙 | TypeScript 작업 시 |
| `instruction.md` | 사용자가 Claude Code에게 작업을 시키는 방법 | 사용자 참고용 |

---

## Claude Code 기본 동작 원칙

Claude Code는 이 저장소에서 다음 원칙을 따른다.

- README를 먼저 읽는다
- CLAUDE의 공통 규칙을 따른다
- 구현 전에는 plan 기준으로 분석한다
- 런타임을 먼저 판단한 뒤 해당 runtime 문서를 읽는다
- 구현은 build 규칙을 따른다
- 구현 후에는 review 기준으로 검토한다
- 결과는 변경 파일, 변경 이유, 테스트 결과, 남은 리스크까지 정리한다

---

## 공통 개발 원칙

모든 Agent는 아래 원칙을 따른다.

- LLM 호출은 LiteLLM Gateway를 우선 사용한다
- 공통 요청/응답 포맷을 유지한다
- Tool 호출 전 입력 검증 및 정책 검증을 수행한다
- 구조화된 로그와 감사 이력을 남긴다
- 테스트 없는 핵심 변경은 하지 않는다
- Agent 토폴로지는 고정하지 않는다
- 기존 구조를 먼저 따른다
- side effect는 코드 구조상 명확히 드러나야 한다
- 설정값, 키, URL을 코드에 하드코딩하지 않는다
- 민감정보를 로그에 남기지 않는다

---

## 공통 요청 / 응답 계약

모든 Agent는 가능한 한 아래 구조를 따른다.

### 요청 구조

```json
{
  "agent_name": "ProjectSummaryAgent",
  "request_id": "REQ-001",
  "session_id": "SES-001",
  "input": {},
  "context": {},
  "metadata": {}
}
```

### 응답 구조

```json
{
  "request_id": "REQ-001",
  "status": "SUCCESS",
  "result": {},
  "errors": [],
  "timings": {
    "total_ms": 0
  }
}
```

### 상태값

- `SUCCESS`
- `PARTIAL_SUCCESS`
- `FAILED`
- `VALIDATION_FAILED`
- `APPROVAL_REQUIRED`
- `BLOCKED`

---

## Python / TypeScript 선택 기준

| 상황 | 우선 선택 |
|---|---|
| RAG, 검색, 임베딩, 상태 기반 복잡 워크플로우 | Python |
| 장기 실행, 체크포인트, 복잡한 orchestration | Python |
| 웹 UI 밀접 통합, streaming 중심 제품 | TypeScript |
| Node.js 서비스와 직접 결합되는 Agent | TypeScript |
| AI 실험, 데이터 처리, 모델 연계 중심 | Python |
| 브라우저/웹앱과 가까운 제품형 Agent | TypeScript |

### 추가 원칙

- TypeScript Agent라고 해서 Next.js를 필수로 사용하지 않는다
- Next.js는 선택적 UI 또는 API Gateway 계층이다
- Python과 TypeScript는 경쟁 관계가 아니다
- 같은 역할의 기능을 두 언어로 중복 구현하지 않는다
- 판단이 애매하면 먼저 `plan.md` 기준으로 분석하고 runtime 문서를 선택한다

---

## LiteLLM 사용 기준

모든 Agent는 LiteLLM Gateway를 공통 경로로 사용한다.

기본 흐름:

Application  
→ LiteLLM Gateway  
→ Model Provider  

### 최소 환경 변수 기준

- `LITELLM_BASE_URL`
- `LITELLM_VIRTUAL_KEY`
- `LITELLM_MODEL`

### 금지사항

- 코드 내부에 API Key 직접 입력
- 모델 직접 호출과 Gateway 호출 혼용
- Provider SDK 직접 연동을 기본 경로로 채택
- 테스트 코드에 실제 운영 키 삽입

---

## 작업 시작 절차

작업을 시작할 때 아래 절차를 따른다.

### 1. 공통 규칙 확인
- `README.md`를 읽는다
- `CLAUDE.md`를 읽는다
- 작업 흐름과 공통 금지사항을 확인한다

### 2. 구현 전 분석
- `plan.md`를 읽는다
- 요구사항을 한 문장으로 다시 정의한다
- 변경 범위를 최소화한다
- 영향 파일, 테스트 영향, 인터페이스 영향, 배포 영향을 확인한다

### 3. 런타임 선택
- Python과 TypeScript 중 적절한 런타임을 선택한다
- 선택한 런타임의 세부 문서를 읽는다

### 4. 구현
- `build.md`를 읽고 구현한다
- 기존 구조를 우선한다
- 테스트를 함께 수정한다
- 민감정보 로그를 금지한다

### 5. 검토
- `review.md`를 읽고 검토한다
- 계약 호환성, LiteLLM 경유, 테스트 누락, 에러 처리, 로깅, 중복 여부를 점검한다

---

## 작업 유형별 빠른 가이드

### 신규 기능 추가

읽을 문서:
- `README.md`
- `CLAUDE.md`
- `plan.md`
- 해당 runtime 문서
- `build.md`
- `review.md`

확인할 것:
- 기능 범위
- 기존 구조 재사용 가능 여부
- 인터페이스 영향
- 테스트 추가 필요성

### 기존 기능 수정

읽을 문서:
- `README.md`
- `CLAUDE.md`
- `plan.md`
- 해당 runtime 문서
- `build.md`
- `review.md`

확인할 것:
- 기존 계약 유지 여부
- 호출자 영향
- 회귀 테스트 범위
- 로그 / 에러 처리 변화 여부

### 버그 수정

읽을 문서:
- `README.md`
- `CLAUDE.md`
- `plan.md`
- 해당 runtime 문서
- `build.md`
- `review.md`

확인할 것:
- 재현 조건
- 직접 영향 파일
- 최소 수정 범위
- 실패 경로 테스트

### Python Agent 작업

반드시 먼저 읽는다.

- `README.md`
- `CLAUDE.md`
- `plan.md`
- `docs/ai/runtime-python.md`
- `build.md`
- `review.md`

핵심 기준:
- LangGraph 우선
- LangChain은 보조 사용
- Pydantic 계약 유지
- FastAPI는 API 레이어
- httpx 사용
- LiteLLM Gateway 유지

### TypeScript Agent 작업

반드시 먼저 읽는다.

- `README.md`
- `CLAUDE.md`
- `plan.md`
- `docs/ai/runtime-typescript.md`
- `build.md`
- `review.md`

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

## 추천 저장소 구조 예시

```text
.
├── README.md
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

## 이 README가 다루지 않는 범위

이 README는 허브 문서다.  
이 README는 세부 구현 규칙을 모두 담지 않는다.

아래 내용은 반드시 개별 문서를 본다.

- 설계 및 영향도 분석 상세 규칙 → `plan.md`
- 구현 행동 규칙 → `build.md`
- 리뷰 및 품질 점검 기준 → `review.md`
- Python 런타임 상세 규칙 → `runtime-python.md`
- TypeScript 런타임 상세 규칙 → `runtime-typescript.md`

---

## Claude Code에게 작업시킬 때의 기본 원칙

Claude Code에게 작업을 시킬 때는 아래 원칙을 따른다.

- README를 먼저 읽게 한다
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

## 최종 행동 규칙

이 README를 읽은 후에는 아래를 바로 실행한다.

- 공통 규칙은 `CLAUDE.md`에서 확인한다
- 구현 전 분석은 `plan.md`에서 수행한다
- 런타임은 목적 기준으로 선택한다
- 구현은 `build.md`를 기준으로 진행한다
- 검토는 `review.md`를 기준으로 수행한다
- Python 작업은 `runtime-python.md`
- TypeScript 작업은 `runtime-typescript.md`

이 README는 시작점이다.  
세부 규칙은 역할에 맞는 문서를 즉시 이어서 읽고 작업한다.
