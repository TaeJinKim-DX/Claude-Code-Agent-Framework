from pathlib import Path

content = """# instruction.md

## 목적

이 문서는 Claude Code 또는 유사 AI 코딩 에이전트가 이 저장소에서 실제 개발 작업을 수행할 때 따라야 하는 **사용자용 실행 가이드**다.

이 문서의 목적은 다음과 같다.

- 사용자가 복잡한 프롬프트를 직접 설계하지 않아도 되게 한다
- Claude Code가 어떤 문서를 어떤 순서로 읽고 어떻게 행동해야 하는지 명확히 한다
- 모든 작업이 분석 → 구현 → 검토 흐름을 따르게 한다
- README와 세부 규칙 문서를 연결하는 **사용자용 작업 지시 허브** 역할을 한다

이 문서는 회사 제출용 문서가 아니다.  
이 문서는 **Claude Code에게 일을 시키는 방법**을 정의한다.

---

# 이 문서만 보면 알아야 하는 것

이 저장소에서 Claude Code에게 일을 시킬 때는 아래 원칙만 기억하면 된다.

- 항상 `README.md`부터 읽게 한다
- 그 다음 `CLAUDE.md`와 역할별 문서를 읽게 한다
- 곧바로 구현시키지 말고 **분석 → 구현 → 검토** 순서로 진행시킨다
- 작업 내용, 제약 조건, 원하는 결과를 같이 준다
- 너무 짧고 모호한 지시는 피한다

즉, Claude Code에게는 항상 아래 흐름으로 지시한다.

`README → plan → runtime → build → review`

---

# Claude Code 기본 동작 흐름

Claude Code는 아래 순서로 작업해야 한다.

## 1. README.md 읽기
가장 먼저 저장소의 README.md를 읽고 프로젝트 전체 흐름을 이해한다.

## 2. CLAUDE.md 읽기
최상위 공통 규칙을 확인한다.

## 3. plan.md 읽기
작업 전 분석, 영향 범위, 리스크를 먼저 파악한다.

## 4. runtime 문서 읽기
작업 대상이 Python인지 TypeScript인지 판단하고 맞는 런타임 문서를 읽는다.

- Python 작업이면 `docs/ai/runtime-python.md`
- TypeScript 작업이면 `docs/ai/runtime-typescript.md`

## 5. build.md 읽기
실제 구현 규칙에 따라 코드를 작성 또는 수정한다.

## 6. review.md 읽기
구현 후 품질, 계약 호환성, 테스트, LiteLLM, 로깅, 정책 위반 여부를 점검한다.

---

# 사용자가 반드시 지켜야 할 지시 원칙

사용자는 Claude Code에게 작업을 시킬 때 아래 원칙을 지킨다.

## 1. README부터 읽게 한다
항상 `README.md를 먼저 읽어`라는 문장을 넣는다.

## 2. 필요한 문서를 추가로 읽게 한다
README만으로 끝내지 말고, 필요한 하위 문서를 읽게 한다.

## 3. 작업 내용을 명확하게 쓴다
“이거 해줘”처럼 모호하게 쓰지 않는다.  
무엇을 바꾸고 싶은지 한 문장으로 명확히 쓴다.

## 4. 제약 조건을 같이 준다
예를 들어 아래와 같은 제약을 같이 준다.

- 기존 인터페이스 유지
- LiteLLM Gateway 유지
- 테스트도 함께 수정
- 민감정보 로그 금지
- 불필요한 리팩토링 금지

## 5. 원하는 결과를 같이 준다
최종적으로 무엇을 보고 싶은지 같이 준다.

예:
- 변경 파일 목록
- 변경 이유
- 테스트 결과
- 남은 리스크

## 6. 가능하면 분석을 먼저 시킨다
바로 구현보다 먼저 `plan 단계만 수행해`라고 지시하는 것이 더 안정적이다.

---

# 가장 추천하는 작업 방식

가장 추천하는 방식은 **3단계 분리 방식**이다.

## 방식 A: 분석 → 구현 → 검토 분리

### 1단계: 분석만 수행
Claude Code에게 먼저 분석만 하게 한다.

예시:

먼저 README.md를 읽고 개발 절차를 따라.  
필요하면 CLAUDE.md, plan.md, runtime 문서를 읽어.  
아직 구현하지 말고 아래 작업에 대해 분석만 수행해.

작업:
[작업 내용]

원하는 결과:
- 요구사항 해석
- 영향 파일
- 인터페이스 영향 여부
- 테스트 영향 범위
- 구현 계획

### 2단계: 구현 수행
분석 결과를 바탕으로 구현하게 한다.

예시:

방금 분석한 결과를 기준으로 구현해.  
README.md, build.md, 해당 runtime 문서 규칙을 따라.

조건:
- 기존 인터페이스 유지
- LiteLLM Gateway 유지
- 테스트 함께 수정
- 불필요한 리팩토링 금지

### 3단계: 검토 수행
구현 후 자체 검토를 시킨다.

예시:

이제 review.md 기준으로 자체 리뷰를 수행해.  
문제 있으면 수정하고, 최종적으로 아래를 정리해.

- 변경 파일 목록
- 변경 이유
- 테스트 결과
- 남은 리스크

---

# 빠르게 한 번에 시키는 방식

한 번에 끝까지 시키고 싶다면 아래 템플릿을 사용한다.

## 통합 실행 템플릿

먼저 README.md를 읽고, README에 정의된 개발 절차를 따라.  
필요하면 아래 문서를 추가로 읽어.

- CLAUDE.md
- .opencode/agents/plan.md
- .opencode/agents/build.md
- .opencode/agents/review.md
- docs/ai/runtime-python.md
- docs/ai/runtime-typescript.md

그 다음 아래 작업을 분석 → 구현 → 리뷰 순서로 진행해.

작업:
[작업 내용]

제약:
- 기존 인터페이스 유지
- LiteLLM Gateway 유지
- 테스트 함께 수정
- 민감정보 로그 금지
- 불필요한 리팩토링 금지

최종 결과:
- 변경 파일 목록
- 변경 이유
- 테스트 결과
- 남은 리스크 또는 후속 작업

---

# 작업 유형별 추천 템플릿

## 1. 신규 기능 추가

먼저 README.md를 읽고 개발 절차를 따라.  
필요하면 CLAUDE.md, plan.md, build.md, review.md, 해당 runtime 문서를 읽어.

아래 신규 기능을 분석 → 구현 → 리뷰 순서로 진행해.

작업:
[새 기능 설명]

조건:
- 기존 구조를 우선 사용
- 기존 인터페이스는 최대한 유지
- LiteLLM Gateway 사용 유지
- 테스트 추가
- 불필요한 리팩토링 금지

최종 결과:
- 설계 요약
- 변경 파일 목록
- 구현 내용 요약
- 테스트 결과
- 남은 리스크

---

## 2. 기존 기능 수정

먼저 README.md를 읽고 개발 절차를 따라.  
그 다음 아래 기능 수정을 분석 → 구현 → 리뷰 순서로 진행해.

작업:
[수정할 기능 설명]

조건:
- 최소 수정 원칙 적용
- 기존 인터페이스 유지
- 테스트 수정
- LiteLLM Gateway 유지

최종 결과:
- 영향 범위 요약
- 변경 파일 목록
- 변경 내용 요약
- 테스트 결과

---

## 3. 버그 수정

먼저 README.md를 읽고 개발 절차를 따라.  
필요한 문서를 읽은 뒤 아래 버그를 분석 → 수정 → 리뷰 순서로 진행해.

버그:
[버그 설명]

조건:
- 재현 조건 먼저 정리
- 최소 수정 원칙 적용
- 기존 인터페이스 유지
- 테스트 추가 또는 수정
- LiteLLM Gateway 우회 금지

최종 결과:
- 원인 분석
- 수정 내용
- 변경 파일 목록
- 테스트 결과
- 회귀 영향 여부

---

## 4. Python Agent 작업

먼저 README.md를 읽고 개발 절차를 따라.  
이 작업은 Python 기반 Agent 작업이다. 아래 문서를 반드시 먼저 읽어.

- CLAUDE.md
- .opencode/agents/plan.md
- docs/ai/runtime-python.md
- .opencode/agents/build.md
- .opencode/agents/review.md

그 다음 아래 작업을 분석 → 구현 → 리뷰 순서로 진행해.

작업:
[Python Agent 작업 내용]

조건:
- LangGraph 우선
- LangChain은 보조 사용
- Pydantic 계약 유지
- FastAPI는 API 레이어 역할만 수행
- httpx 사용
- LiteLLM Gateway 유지

최종 결과:
- 설계 요약
- 변경 파일 목록
- 테스트 결과
- 남은 리스크

---

## 5. TypeScript Agent 작업

먼저 README.md를 읽고 개발 절차를 따라.  
이 작업은 TypeScript 기반 Agent 작업이다. 아래 문서를 반드시 먼저 읽어.

- CLAUDE.md
- .opencode/agents/plan.md
- docs/ai/runtime-typescript.md
- .opencode/agents/build.md
- .opencode/agents/review.md

그 다음 아래 작업을 분석 → 구현 → 리뷰 순서로 진행해.

작업:
[TypeScript Agent 작업 내용]

조건:
- Node.js 기반 Agent 구조 유지
- LangGraph JS 우선
- LangChain.js는 보조 사용
- Zod 계약 유지
- LiteLLM Gateway 유지
- Next.js는 UI 계층일 때만 사용

최종 결과:
- 설계 요약
- 변경 파일 목록
- 테스트 결과
- 남은 리스크

---

## 6. 리팩토링 작업

먼저 README.md를 읽고 개발 절차를 따라.  
아래 리팩토링 작업을 분석 → 구현 → 리뷰 순서로 진행해.

대상:
[리팩토링 대상]

조건:
- 동작 변경 금지
- 기존 인터페이스 유지
- 테스트 유지
- 불필요한 추상화 금지
- 중복 제거 우선

최종 결과:
- 변경 파일 목록
- 리팩토링 이유
- 테스트 결과
- 구조 개선 요약

---

# Claude Code에게 이렇게 시키면 안 되는 예시

아래처럼 지시하면 안 된다.

## 나쁜 예시 1
코드 좀 고쳐줘

## 나쁜 예시 2
대충 알아서 구현해줘

## 나쁜 예시 3
이거 개선해줘

## 나쁜 예시 4
README 읽고 실행해줘

이런 지시는 작업 범위, 제약 조건, 원하는 결과가 없어서 품질이 불안정해진다.

---

# Claude Code에게 지시할 때 반드시 포함할 것

항상 아래 4가지를 포함한다.

## 1. 작업 내용
무엇을 바꾸고 싶은지 명확히 쓴다.

## 2. 제약 조건
무엇을 지켜야 하는지 같이 쓴다.

예:
- 기존 인터페이스 유지
- LiteLLM Gateway 유지
- 테스트 수정
- 민감정보 로그 금지

## 3. 참조 문서
README.md와 필요한 하위 문서를 읽게 한다.

## 4. 원하는 결과
최종적으로 무엇을 출력해야 하는지 적는다.

예:
- 변경 파일 목록
- 변경 이유
- 테스트 결과
- 남은 리스크

---

# Claude Code 작업 전 사용자 체크리스트

Claude Code에게 지시하기 전에 사용자는 아래를 확인한다.

- 작업 목적을 한 문장으로 설명할 수 있는가
- 신규 기능인지, 수정인지, 버그인지 구분했는가
- Python 작업인지 TypeScript 작업인지 대략 판단했는가
- 인터페이스를 깨면 안 되는지 알고 있는가
- 테스트를 같이 수정해야 하는지 알고 있는가
- 원하는 결과를 정리했는가

---

# 가장 추천하는 최종 템플릿

아래 템플릿을 가장 추천한다.  
애매하면 이것만 복사해서 `[작업 내용]`만 바꾸면 된다.

먼저 README.md를 읽고, README에 정의된 개발 절차를 따라.  
필요하면 아래 문서를 추가로 읽어.

- CLAUDE.md
- .opencode/agents/plan.md
- .opencode/agents/build.md
- .opencode/agents/review.md
- docs/ai/runtime-python.md
- docs/ai/runtime-typescript.md

그 다음 아래 작업을 분석 → 구현 → 리뷰 순서로 진행해.

작업:
[작업 내용]

제약:
- 기존 인터페이스 유지
- LiteLLM Gateway 유지
- 테스트 함께 수정
- 민감정보 로그 금지
- 불필요한 리팩토링 금지

최종 결과:
- 변경 파일 목록
- 변경 이유
- 테스트 결과
- 남은 리스크 또는 후속 작업

---

# 최종 원칙

이 저장소에서 Claude Code를 사용할 때는 항상 아래 흐름을 따른다.

README  
→ CLAUDE  
→ plan  
→ runtime  
→ build  
→ review

이 순서를 지키면 Claude Code가 가장 안정적으로 개발을 진행한다.
"""

path = Path("/mnt/data/instruction.md")
path.write_text(content, encoding="utf-8")
print(f"Saved to {path}")
