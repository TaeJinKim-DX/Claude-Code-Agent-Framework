## 문서 목적

이 문서는 TypeScript 기반 Agent를 구현할 때 따라야 하는 런타임 및 구현 규칙을 정의한다.  
이 문서는 프로젝트 전체 공통 규칙 문서가 아니다.  
이 문서는 Python 런타임 문서가 아니다.  
이 문서는 TypeScript Agent 구현에 필요한 언어 및 런타임 특화 규칙만 다룬다.

이 문서의 목적은 다음과 같다.

- TypeScript Agent 구현 시 일관된 런타임 기준을 유지한다.
- Node.js 기반 Agent 구조를 안정적으로 유지한다.
- LangGraph JS / LangChain.js 사용 기준을 분명히 한다.
- Zod, Fastify, Express, Hono, Next.js의 역할을 구분한다.
- 테스트, 디렉터리 구조, 금지사항을 명확히 한다.
- 이 문서 하나만 반복 참조해도 TypeScript Agent 구현 품질을 안정적으로 유지한다.

---

## 적용 범위

이 문서는 다음 작업에 적용한다.

- TypeScript 기반 Agent 신규 구현
- TypeScript 기반 Agent 수정
- Node.js 기반 Agent 서비스 구현
- LangGraph JS 기반 워크플로우 구현
- LangChain.js + LangGraph JS 조합 구현
- Zod 기반 요청/응답 스키마 정의
- Fastify / Express / Hono 기반 API 작성
- Next.js 기반 UI 또는 API Gateway 연계
- TypeScript 테스트 작성 및 수정

이 문서는 다음 내용을 직접 다루지 않는다.

- Python 런타임 규칙
- 리뷰 전용 체크리스트
- 프로젝트 전체 공통 규칙 반복 설명
- 설계 분석 절차
- 특정 비즈니스 도메인 규칙

---

## TypeScript 기술 스택 및 기준 버전

| 구분 | 제품/패키지 | 기준 버전 | 용도 |
|---|---|---:|---|
| LLM Gateway | LiteLLM | 1.82.1 | 다중 모델 라우팅 및 게이트웨이 |
| Runtime | Node.js | 24.14.x (LTS) 또는 프로젝트 확정 버전 | TypeScript Agent 실행 런타임 |
| Language | TypeScript | 프로젝트 확정 버전 | Agent 개발 언어 |
| Client Framework | Next.js | 16.1.x | TypeScript 기반 Agent UI 또는 API Gateway 계층 |
| Agent Framework | LangGraph JS | 프로젝트 확정 버전 | 상태 기반 워크플로우 및 Agent 오케스트레이션 |
| Agent Framework | LangChain.js + LangGraph JS | 프로젝트 확정 버전 | Tool 연동과 상태 기반 오케스트레이션 |
| Validation | Zod | 프로젝트 확정 버전 | 요청/응답 및 Tool schema 검증 |
| Server Framework | Hono | 4.12.x | 경량 TypeScript Agent API 제공 |
| Server Framework | Fastify | 5.8.x | TypeScript Agent API 제공 |

### 버전 관리 규칙

- dependency는 위 표의 기준 버전을 따른다.
- `package.json` 생성 시 버전 범위를 자동으로 느슨하게 생성하지 않는다.
- 특별한 사유가 없으면 lock 파일 기준으로 고정 관리한다.
- 표준 문서와 다른 버전을 사용할 경우 사유를 남긴다.
- 메이저 버전 변경 시 회귀 테스트를 수행한다.

---

## TypeScript Agent 적용 대상

아래 경우 TypeScript 기반 Agent를 우선 검토한다.

| 상황 | TypeScript 우선 여부 |
|---|---|
| 웹 제품에 직접 연결되는 Agent | 우선 |
| Chat UI, Copilot UI, SaaS형 Agent | 우선 |
| Streaming 응답 중심 Agent | 우선 |
| Node.js 서비스와 직접 결합되는 Agent | 우선 |
| 브라우저/웹앱과 가까운 제품형 Agent | 우선 |
| RAG, 검색, 임베딩 중심 Agent | 보류 후 비교 |
| 상태 기반 복잡 워크플로우 | 보류 후 비교 |

### 기본 판단 규칙
- 웹 제품 통합과 UI 근접 기능이 핵심이면 TypeScript를 우선한다.
- Node.js 런타임 안에서 Agent를 제공해야 하면 TypeScript를 우선한다.
- Next.js는 선택적 UI 계층일 뿐, Agent 자체의 기본 프레임워크로 강제하지 않는다.
- 상태 기반 복잡 orchestration이 매우 크면 Python 문서도 함께 비교한다.

---

## 핵심 원칙

### 1. Node.js 기반 Agent로 구현한다
TypeScript Agent는 Node.js 런타임에서 실행되는 백엔드 Agent로 본다.  
브라우저 코드와 Agent 핵심 로직을 혼동하지 않는다.

### 2. LangGraph JS를 기본 오케스트레이션으로 사용한다
상태 기반 흐름, 분기, 재시도, Tool orchestration이 필요한 경우 LangGraph JS를 우선한다.

### 3. LangChain.js는 보조적으로 사용한다
LangChain.js는 Tool abstraction, 모델 래핑, structured handling이 필요할 때 사용한다.  
핵심 실행 구조를 LangChain.js 단독 loop에만 의존하지 않는다.

### 4. Zod로 계약을 고정한다
요청, 응답, Tool schema, 설정 구조는 가능한 한 Zod로 명시한다.  
핵심 계약을 타입 없는 객체로만 유지하지 않는다.

### 5. API 계층과 Agent 계층을 분리한다
Fastify, Express, Hono는 API 계층이다.  
Agent 판단 로직을 라우터 내부에 과도하게 넣지 않는다.

### 6. Next.js는 선택적 계층으로 사용한다
Next.js는 UI 또는 API Gateway 계층으로 사용할 수 있다.  
Next.js를 Agent 핵심 런타임으로 전제하지 않는다.

### 7. LiteLLM Gateway를 우선한다
TypeScript Agent도 Python Agent와 동일하게 LiteLLM Gateway를 기본 경로로 사용한다.

---

## Node.js 기반 원칙

### 반드시 지켜야 할 규칙
- TypeScript Agent는 Node.js 서버 환경을 기본으로 구현한다.
- 브라우저 전용 코드와 서버 전용 코드를 분리한다.
- 환경변수와 런타임 설정을 명시적으로 관리한다.
- Agent 핵심 로직은 UI 레이어와 분리한다.
- 요청 처리, Tool 호출, 상태 관리, 응답 생성의 책임을 분리한다.

### 금지사항
- UI 이벤트 핸들러에 Agent 핵심 로직을 직접 구현
- 브라우저 전용 코드에서 민감한 Gateway 설정 직접 사용
- Next.js 페이지 또는 라우트 파일에 모든 Agent 흐름 구현
- Node.js 서비스와 프론트엔드 컴포넌트 로직을 혼합

---

## LangGraph JS / LangChain.js 사용 기준

### LangGraph JS를 우선 사용하는 경우
- 상태 기반 분기 흐름이 있다
- Tool 호출이 여러 단계로 이어진다
- 재시도, 중단, 재개가 필요하다
- 실행 흐름을 명시적으로 보여야 한다

### LangChain.js를 함께 사용하는 경우
- Tool schema와 모델 abstraction이 필요하다
- LangChain.js 제공 wrapper가 생산성을 높인다
- LangGraph JS 내부 노드에서 모델/Tool 계층을 정리할 필요가 있다

### LangChain.js 단독 사용을 검토할 수 있는 경우
- 흐름이 매우 단순하다
- 상태 기반 분기 필요성이 낮다
- Tool 호출과 응답 생성이 짧은 루프로 충분하다

### 기본 원칙
- 오케스트레이션은 LangGraph JS를 우선한다.
- LangChain.js는 보조 라이브러리로 사용한다.
- LangChain.js가 구조 전체를 지배하지 않게 한다.
- 추후 LangGraph JS로 확장 가능한 구조를 우선한다.

---

## Zod 사용 원칙

### 반드시 지켜야 할 규칙
- 요청 스키마는 Zod로 정의한다.
- 응답 스키마는 Zod로 정의한다.
- Tool 입력/출력 스키마도 가능하면 Zod로 정의한다.
- 자유형 object는 경계 계층에서만 제한적으로 사용한다.
- validation 없이 Agent 핵심 로직으로 진입하지 않는다.

### 금지사항
- 핵심 계약을 타입만 있고 런타임 검증 없는 상태로 유지
- any 기반 객체를 핵심 흐름에 직접 전달
- request/response 구조를 암묵적으로 해석
- Tool 입력 구조를 validation 없이 처리

---

## Fastify / Express / Hono 선택 기준

| 서버 프레임워크 | 권장 상황 |
|---|---|
| Fastify | 성능, 타입 연계, 구조적 API 구성이 중요할 때 |
| Express | 기존 생태계 또는 단순 Node 서비스와의 호환이 중요할 때 |
| Hono | 경량 구조, 간결한 API, edge 또는 단순 서비스가 필요할 때 |

### 선택 규칙
- 기본 선택은 Fastify를 우선 검토한다.
- 기존 서비스가 Express 기반이면 무리한 전환보다 일관성을 우선한다.
- 경량 API 또는 단순 Gateway 성격이면 Hono를 검토한다.
- 서버 프레임워크 선택은 API 계층 선택이지 Agent 구조 선택이 아니다.

### 금지사항
- 프레임워크 선호만으로 전체 구조 변경
- API 서버와 Agent 구조를 동일 개념으로 취급
- 한 저장소 안에서 특별한 이유 없이 여러 서버 프레임워크 혼용

---

## Next.js 사용 기준

### Next.js를 사용하는 경우
- Agent UI가 필요하다
- Chat 인터페이스가 필요하다
- API Gateway 역할을 같이 수행한다
- 웹 제품 안에서 Agent를 직접 노출한다

### Next.js를 사용하지 않는 경우
- 백엔드 Agent만 제공한다
- UI와 Agent를 분리 운영한다
- Node API 서버만 있으면 충분하다

### 원칙
- Next.js는 선택적 UI 계층이다.
- Next.js는 TypeScript Agent의 필수 요소가 아니다.
- Agent 핵심 로직은 Next.js 컴포넌트 계층 안에 넣지 않는다.
- API Route를 사용하더라도 Agent 핵심 실행 계층은 분리한다.

---

## 표준 디렉터리 구조

```text
agent-ts/
 ├── agent/
 │   ├── core/
 │   ├── orchestration/
 │   ├── providers/
 │   ├── adapters/
 │   ├── policies/
 │   ├── logging/
 │   └── exceptions/
 ├── tools/
 ├── api/
 ├── service/
 ├── schemas/
 ├── scripts/
 ├── tests/
 ├── deploy/
 ├── infra/
 └── docs/
