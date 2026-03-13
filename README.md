from pathlib import Path

content = """# README.md

## 프로젝트 개요

이 저장소는 AI Agent 플랫폼 및 서비스 개발을 위한 저장소다.  
Python 기반 Agent와 TypeScript 기반 Agent를 모두 수용한다.  
LLM 호출은 LiteLLM Gateway를 공통 경로로 사용한다.  
이 README는 소개 문서가 아니라 **개발 실행 허브 문서**다.

이 저장소에서 작업하는 AI 코딩 에이전트는 이 README를 먼저 읽고,  
작업 유형에 따라 필요한 규칙 문서를 추가로 읽은 뒤 작업을 진행한다.

---

## 이 README의 역할

이 README는 다음 역할만 수행한다.

- 프로젝트 개발 진입점 제공
- 작업 시작 순서 정의
- 문서별 역할 안내
- Python / TypeScript 선택 기준 요약
- 구현 전후 기본 체크리스트 제공
- 금지사항 요약 제공

이 README는 다음을 직접 대체하지 않는다.

- 상세 설계 분석 규칙
- 상세 구현 규칙
- 상세 리뷰 규칙
- Python 저수준 구현 규칙
- TypeScript 저수준 구현 규칙

세부 규칙은 별도 문서를 참조한다.

---

## 가장 먼저 읽을 문서

작업을 시작할 때는 아래 순서를 따른다.

1. `CLAUDE.md`
2. `.opencode/agents/plan.md`
3. 작업 런타임에 맞는 문서
   - Python 작업: `docs/ai/runtime-python.md`
   - TypeScript 작업: `docs/ai/runtime-typescript.md`
4. `.opencode/agents/build.md`
5. 작업 완료 후 `.opencode/agents/review.md`

---

## 문서 역할 요약

| 문서 | 역할 | 언제 읽는가 |
|---|---|---|
| `CLAUDE.md` | 프로젝트 전체 최상위 공통 규칙 | 모든 작업 시작 전 |
| `.opencode/agents/plan.md` | 설계 및 영향도 분석 기준 | 구현 전 |
| `.opencode/agents/build.md` | 실제 코드 작성/수정 규칙 | 구현 중 |
| `.opencode/agents/review.md` | 리뷰, 품질 점검, 리팩토링 검토 기준 | 구현 후 |
| `docs/ai/runtime-python.md` | Python Agent 런타임/구현 규칙 | Python 작업 시 |
| `docs/ai/runtime-typescript.md` | TypeScript Agent 런타임/구현 규칙 | TypeScript 작업 시 |

---

## 작업 시작 절차

작업을 시작할 때 아래 절차를 따른다.

### 1. 공통 규칙 확인
- `CLAUDE.md`를 읽는다.
- LiteLLM Gateway 우선 원칙을 확인한다.
- 공통 인터페이스 유지 원칙을 확인한다.
- 금지사항을 확인한다.

### 2. 구현 전 분석
- `plan.md`를 읽는다.
- 요구사항을 한 문장으로 다시 정의한다.
- 변경 범위를 최소화한다.
- 영향 파일, 테스트 영향, 인터페이스 영향, 배포 영향 여부를 확인한다.

### 3. 런타임 선택
- Python과 TypeScript 중 적절한 런타임을 선택한다.
- 선택한 런타임의 상세 규칙 문서를 읽는다.

### 4. 구현
- `build.md`를 읽고 구현한다.
- 기존 구조를 우선한다.
- 테스트를 함께 수정한다.
- 민감정보 로그를 금지한다.

### 5. 검토
- `review.md`를 읽고 검토한다.
- 계약 호환성, LiteLLM 경유, 테스트 누락, 에러 처리, 로깅, 중복 구현 여부를 점검한다.

---

## 런타임 선택 기준

### Python을 우선하는 경우
- RAG, 검색, 임베딩 중심 Agent
- 상태 기반 복잡한 워크플로우
- 체크포인트, 재시도, 장기 실행
- Tool orchestration이 복잡한 Agent
- 데이터 처리 및 AI 로직 중심 기능

### TypeScript를 우선하는 경우
- 웹 제품에 직접 연결되는 Agent
- Chat UI, Copilot UI, SaaS형 Agent
- Streaming 응답 중심 Agent
- Node.js 서비스와 직접 결합되는 Agent
- 브라우저/웹앱과 가까운 제품형 Agent

### 추가 원칙
- TypeScript Agent라고 해서 Next.js를 필수로 사용하지 않는다.
- Next.js는 선택적 UI 또는 API Gateway 계층이다.
- Python과 TypeScript는 경쟁 관계가 아니다.
- 같은 역할의 기능을 두 언어로 중복 구현하지 않는다.

---

## 공통 개발 원칙

이 저장소에서 모든 Agent는 아래 원칙을 따른다.

- LLM 호출은 LiteLLM Gateway를 우선 사용한다.
- 공통 요청/응답 포맷을 유지한다.
- Tool 호출 전 정책 검증을 수행한다.
- 구조화된 로그와 감사 이력을 남긴다.
- 테스트 없는 핵심 변경은 하지 않는다.
- Agent 토폴로지는 고정하지 않는다.
- 기존 구조를 먼저 따른다.
- side effect는 코드 구조상 명확히 드러나야 한다.
- 설정값, 키, URL을 코드에 하드코딩하지 않는다.
- 민감정보를 로그에 남기지 않는다.

---

## 공통 요청/응답 계약

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
