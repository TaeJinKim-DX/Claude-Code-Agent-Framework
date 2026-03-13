## 문서 목적

이 문서는 Python 기반 Agent를 구현할 때 따라야 하는 런타임 및 구현 규칙을 정의한다.  
이 문서는 프로젝트 전체 공통 규칙 문서가 아니다.  
이 문서는 TypeScript 런타임 문서가 아니다.  
이 문서는 Python Agent 구현에 필요한 언어 및 런타임 특화 규칙만 다룬다.

이 문서의 목적은 다음과 같다.

- Python Agent 구현 시 일관된 런타임 기준을 유지한다.
- LangGraph 중심의 상태 기반 Agent 구조를 우선 적용한다.
- LangChain + LangGraph 조합 사용 기준을 분명히 한다.
- Pydantic, FastAPI, httpx 사용 원칙을 통일한다.
- 테스트, 디렉터리 구조, 금지사항을 명확히 한다.
- 이 문서 하나만 반복 참조해도 Python Agent 구현 품질을 안정적으로 유지한다.

---

## 적용 범위

이 문서는 다음 작업에 적용한다.

- Python 기반 Agent 신규 구현
- Python 기반 Agent 수정
- LangGraph 기반 워크플로우 구현
- LangChain + LangGraph 조합 구현
- FastAPI 기반 Agent API 작성
- Pydantic 모델 정의
- httpx 기반 외부 통신 구현
- Python 테스트 작성 및 수정

이 문서는 다음 내용을 직접 다루지 않는다.

- TypeScript 런타임 규칙
- 리뷰 전용 체크리스트
- 프로젝트 전체 공통 규칙 반복 설명
- 설계 분석 절차
- 특정 비즈니스 도메인 규칙

---

## Python Agent 적용 대상

아래 경우 Python 기반 Agent를 우선 검토한다.

| 상황 | Python 우선 여부 |
|---|---|
| RAG, 검색, 임베딩 중심 Agent | 우선 |
| 상태 기반 복잡한 워크플로우 | 우선 |
| 체크포인트, 재시도, 장기 실행 | 우선 |
| Tool orchestration이 복잡한 Agent | 우선 |
| 모델 연계, 데이터 처리, 실험성 로직 | 우선 |
| 웹 UI와 직접 결합되는 단순 제품형 Agent | 보류 후 비교 |

### 기본 판단 규칙
- 상태 기반 orchestration이 중요하면 Python을 우선한다.
- RAG / 검색 / 데이터 연계가 핵심이면 Python을 우선한다.
- 단순 웹 채팅 UI 자체는 Python 선택 근거가 아니다.
- Node 서비스와의 직접 결합이 핵심이면 TypeScript 문서를 본다.

---

## 핵심 원칙

### 1. LangGraph를 기본 오케스트레이션으로 사용한다
Python Agent의 기본 오케스트레이션은 LangGraph를 우선한다.  
단순 선형 흐름이라도 향후 확장 가능성이 있으면 LangGraph 구조를 먼저 검토한다.

### 2. LangChain은 보조적 도구로 사용한다
LangChain은 Tool 호출, 모델 래핑, structured output, 일부 유틸리티가 필요한 경우에 사용한다.  
Agent의 핵심 실행 구조를 LangChain 단독 체인 위주로 설계하지 않는다.

### 3. 상태를 명시적으로 관리한다
입력, 중간 상태, Tool 결과, 최종 결과를 명시적으로 다룬다.  
암묵적 전역 상태나 숨은 side effect에 의존하지 않는다.

### 4. Pydantic으로 계약을 고정한다
요청, 응답, 설정, Tool 입출력 구조는 가능한 한 Pydantic 모델로 정의한다.  
자유형 dict만으로 핵심 계약을 유지하지 않는다.

### 5. 외부 통신은 일관된 클라이언트로 처리한다
외부 HTTP 통신은 httpx를 기본으로 사용한다.  
여러 HTTP 클라이언트를 혼용하지 않는다.

### 6. API 레이어와 Agent 로직을 분리한다
FastAPI는 입력 수신과 응답 반환에 집중한다.  
Agent 판단 로직을 라우터나 엔드포인트 함수에 과도하게 넣지 않는다.

---

## LangGraph 중심 원칙

### 반드시 지켜야 할 규칙
- 상태 기반 흐름이 있는 경우 LangGraph를 기본으로 설계한다.
- 분기, 재시도, 승인, 중단/재개 가능성이 있으면 LangGraph 우선이다.
- Tool 호출 흐름이 2단계 이상이면 LangGraph 사용을 먼저 검토한다.
- 노드 책임은 작고 명확하게 유지한다.
- 상태 스키마를 명확히 정의한다.
- 노드 간 데이터 전달은 상태를 통해 명시적으로 한다.

### 피해야 할 구현
- 거대한 단일 함수에 전체 흐름을 모두 넣는 구조
- hidden global state 사용
- 노드 책임이 불명확한 그래프
- 분기 조건이 코드 여러 곳에 흩어진 구조

---

## LangChain + LangGraph 사용 기준

### 사용하는 경우
- Tool schema와 structured output 활용이 필요한 경우
- 모델 래퍼를 일관되게 쓰고 싶은 경우
- LangChain 제공 유틸리티가 생산성을 높이는 경우
- LangGraph 내부 노드에서 LangChain 모델/Tool abstraction을 쓰는 경우

### 사용하지 않는 경우
- 단순 체인만으로 전체 Agent를 유지하려는 경우
- 상태 기반 복잡한 흐름인데 LangGraph 없이 LangChain loop만으로 해결하려는 경우
- 공통 계약보다 빠른 실험 코드를 우선하는 경우

### 원칙
- 오케스트레이션은 LangGraph를 우선한다.
- Tool 및 모델 abstraction은 필요 시 LangChain을 사용한다.
- LangChain이 구조를 주도하지 않게 한다.
- LangChain 사용은 편의성 확보를 위한 것이지 구조 대체가 아니다.

---

## Pydantic 사용 원칙

### 반드시 지켜야 할 규칙
- 요청 모델은 Pydantic으로 정의한다.
- 응답 모델은 Pydantic으로 정의한다.
- 환경설정 모델도 가능한 한 구조화한다.
- Tool 입력/출력도 핵심 계약이면 Pydantic 모델을 우선한다.
- 자유형 dict는 경계 계층에서만 제한적으로 사용한다.

### 금지사항
- 핵심 계약을 타입 없는 dict로만 유지
- request/response 구조를 문자열 파싱으로 해석
- 필수 필드와 선택 필드를 모호하게 두는 모델
- validation 없이 Agent 핵심 로직으로 진입

---

## FastAPI 사용 원칙

### 반드시 지켜야 할 규칙
- FastAPI는 API 레이어로 사용한다.
- 엔드포인트는 입력 검증, 서비스 호출, 응답 변환에 집중한다.
- 비즈니스 로직과 Agent 실행 흐름은 서비스/agent 계층으로 분리한다.
- 요청/응답 모델은 Pydantic으로 명시한다.
- endpoint 함수에 직접 LLM 호출 로직을 길게 넣지 않는다.

### 권장 구조
```text
api/
service/
agent/
tools/
models/
