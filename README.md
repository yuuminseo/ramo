<div align="center">
  <img src="frontend/public/ramo-logo.png" width="150" alt="RAMO 로고" />
  <h1>RAMO</h1>
  <p><strong>LLM과의 비선형 대화를 브랜치와 그래프로 관리하는 채팅 서비스</strong></p>
  <p>
    <img src="https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB" alt="React" />
    <img src="https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white" alt="Vite" />
    <img src="https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white" alt="FastAPI" />
    <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL" />
    <img src="https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white" alt="Vercel" />
    <img src="https://img.shields.io/badge/Railway-0B0D0E?style=flat-square&logo=railway&logoColor=white" alt="Railway" />
  </p>
  <p>
    <a href="https://ramo-pi.vercel.app"><strong>서비스 실행</strong></a>
    &nbsp;&middot;&nbsp;
    <a href="https://github.com/uiuuymin/ramo"><strong>백엔드 저장소</strong></a>
    &nbsp;&middot;&nbsp;
    <a href="https://chatbotbranchproject-production.up.railway.app/docs"><strong>API 문서</strong></a>
  </p>
</div>

---

## 프로젝트 소개

RAMO는 LLM과 대화하는 과정에서 생기는 꼬리질문과 주제 전환을 별도의 브랜치로 분리하고, 대화 간 관계를 그래프로 시각화하는 웹 서비스이다.

기존 채팅 서비스에서는 하나의 대화가 선형으로 쌓이기 때문에 중간에 다른 주제를 탐색하면 원래 맥락이 흐려진다. 새 채팅을 만들면 기존 맥락을 다시 설명해야 하고, 여러 대화로 분기한 뒤에는 각 대화의 관계를 기억하기도 어렵다.

RAMO는 대화를 노드 단위로 관리한다. 사용자는 원하는 답변에서 새 브랜치를 만들고, 각 노드의 관계와 현재 위치를 그래프에서 확인하며, 여러 갈래로 탐색한 결과를 다시 하나의 노드로 병합할 수 있다.

```text
질문과 답변
→ 원하는 지점에서 브랜치 생성
→ 그래프로 대화 구조 확인
→ 서로 다른 방향으로 탐색
→ 답변 비교 또는 노드 병합
→ 정리된 맥락에서 대화 계속
```

## 팀 구성

2026년 1학기 KHUDA 심화 프로젝트 NLP-AIE 3팀에서 개발했다.

| 팀원 |
| --- |
| 표지훈 · 김민영 · 유민서 · 이시현 · 정시찬 · 양경식 |

## 문제 의식 🚨

- LLM과의 작업은 비선형적으로 진행되지만 기존 채팅 화면은 선형적인 목록과 스크롤로 구성된다.
- 대화 중 발생한 꼬리질문이 누적되면 현재 질문과 관계없는 내용이 컨텍스트에 포함된다.
- 새 채팅으로 분리하면 기존 맥락을 다시 설명해야 하며, 분기된 대화 간 관계도 한눈에 파악하기 어렵다.
- 여러 모델의 답변을 비교하거나 서로 다른 탐색 결과를 하나로 수렴시키는 과정이 번거롭다.

## 해결 방안 ☺️

### 기존 방식

1. 하나의 긴 대화에서 모든 질문을 이어간다.
2. 이전 질문을 수정하거나 답변을 다시 생성한다.
3. 새 채팅을 만들고 필요한 맥락을 다시 입력한다.
4. 중요한 답변을 별도의 메모장이나 문서에 복사해 관리한다.

위 방식은 대화가 길어질수록 현재 맥락을 찾는 비용과 사용자가 기억해야 하는 정보가 증가한다.

### RAMO

- AI 답변을 기준으로 새로운 하위 노드를 생성한다.
- 하위 노드는 분기 지점까지의 상위 대화 맥락을 이어받는다.
- 전체 대화 구조와 현재 위치를 그래프로 시각화한다.
- 필요 없는 노드는 비활성화하거나 휴지통으로 이동할 수 있다.
- 서로 다른 가지의 노드를 병합해 탐색 결과를 하나의 요약으로 정리한다.
- 여러 LLM의 답변을 동시에 비교하고 선택하거나 융합한다.

## 핵심 기능

### 1. 브랜치 기반 대화 관리

AI 답변에서 새로운 브랜치를 생성할 수 있다. 새 브랜치에서는 분기 지점까지의 상위 대화를 참고하므로 기존 맥락을 다시 설명하지 않고 새로운 방향을 탐색할 수 있다.

### 2. 대화 그래프 시각화

루트 노드, 하위 노드, 현재 노드와 main 경로를 그래프로 표시한다. 사용자는 미니 그래프 또는 전체화면 그래프에서 노드를 선택해 원하는 대화로 바로 이동할 수 있다.

### 3. 노드 병합

서로 다른 가지에 있는 두 노드를 선택해 하나의 병합 노드를 생성한다. 병합 결과에는 두 대화의 핵심 내용이 요약되며, 그래프에는 두 부모 노드와의 연결 관계가 유지된다.

### 4. 모델 답변 비교와 융합

같은 프롬프트에 대한 여러 모델의 답변을 나란히 비교한다. 답변 분석 결과를 확인한 뒤 하나를 선택하거나, 장점을 결합한 융합 답변을 현재 대화에 반영할 수 있다.

### 5. 노드별 맥락 관리

노드 이름과 설명을 대화 내용에 맞게 관리하고, main 경로를 지정해 중심 흐름을 표시한다. 노드별 페르소나, 태그, 파일 첨부 기능도 제공한다.

### 6. 대화 생애주기 관리

더 이상 필요하지 않은 대화는 비활성화하거나 접을 수 있다. 세션을 휴지통으로 이동한 뒤 복구하거나 영구 삭제하는 흐름도 지원한다.

## 기대 효과

| 효과 | 설명 |
| --- | --- |
| 컨텍스트 오염 감소 | 서로 다른 주제를 별도 브랜치로 분리해 현재 질문과 관련 없는 과거 맥락의 영향을 줄인다. |
| 탐색 시간 감소 | 긴 대화를 다시 스크롤하지 않고 그래프에서 분기 지점과 원하는 대화를 찾는다. |
| 맥락 재사용 | 기존 노드에서 분기해 앞선 내용을 반복해서 입력하지 않는다. |
| 외부 기억 장치 | 사용자가 기억하던 대화 관계를 그래프가 대신 표현한다. |
| 탐색 결과 수렴 | 분리된 대화를 병합하거나 모델 답변을 융합해 하나의 흐름으로 정리한다. |

프로젝트 발표자료의 예시 시나리오에서는 브랜치 사용 시 토큰 사용량이 `655.14`에서 `542.23`으로 줄어 기존 방식 대비 약 `17.1%` 감소하는 결과를 확인했다. 해당 수치는 통제된 시나리오 기반 시뮬레이션이며, 실제 사용 환경에서의 절감률은 대화 구조와 모델별 컨텍스트 정책에 따라 달라질 수 있다.

## 시스템 아키텍처

```text
┌──────────────────────────────┐
│ React + Vite Frontend        │
│ Vercel                      │
└──────────────┬───────────────┘
               │ REST API
┌──────────────▼───────────────┐
│ FastAPI Backend              │
│ Railway                      │
├──────────────────────────────┤
│ Session / Branch / Graph     │
│ Merge / Comparison / Files   │
│ Persona / Tag / Trash        │
└──────────┬───────────┬───────┘
           │           │
┌──────────▼───┐  ┌────▼─────────────────┐
│ PostgreSQL   │  │ OpenAI / ChatKHU LLM │
└──────────────┘  └──────────────────────┘
```

| 구성 요소 | 책임 |
| --- | --- |
| React 프론트엔드 | 채팅 UI, 브랜치 그래프, 노드 병합, 모델 비교, 파일 첨부 화면을 제공한다. |
| FastAPI 백엔드 | 세션·메시지·브랜치 저장, 컨텍스트 구성, 그래프 생성, LLM 호출을 처리한다. |
| PostgreSQL | 세션, 브랜치, 메시지, 태그, 페르소나와 파일 메타데이터를 저장한다. |
| Vercel | 프론트엔드 정적 빌드와 웹 서비스를 배포한다. |
| Railway | FastAPI 서버와 PostgreSQL 실행 환경을 제공한다. |

프론트엔드와 백엔드는 독립적으로 배포하고 있다.

- Frontend: [JihunPyo/ramo](https://github.com/JihunPyo/ramo)
- Backend: [uiuuymin/ramo](https://github.com/uiuuymin/ramo)

## 기술 스택

| 구분 | 기술 |
| --- | --- |
| Frontend | React 19, Vite 8, JavaScript, CSS, SVG, KaTeX |
| Backend | FastAPI, Python, SQLAlchemy, Uvicorn |
| Database | PostgreSQL, SQLite 개발 환경 지원 |
| LLM | OpenAI API, ChatKHU 모델 게이트웨이 |
| Deployment | Vercel, Railway |

## Quick Start

Mock API를 사용하면 별도의 백엔드와 API 키 없이 프론트엔드 기능을 확인할 수 있다.

```bash
git clone https://github.com/JihunPyo/ramo.git
cd ramo/frontend

npm install
npm run dev:mock
```

브라우저에서 `http://127.0.0.1:5173`으로 접속한다.

실제 백엔드와 연결하려면 `frontend/.env.development`에 API 주소를 설정한다.

```env
VITE_API_BASE_URL=http://127.0.0.1:8000
VITE_USE_MOCK_API=false
```

```bash
cd frontend
npm run dev
```

프론트엔드 개발 환경은 Node.js 24와 npm 11 이상을 기준으로 한다.

## 주요 명령어

```bash
# 개발 서버
npm run dev

# Mock API 개발 서버
npm run dev:mock

# ESLint 검사
npm run lint

# 프로덕션 빌드
npm run build
```

## 프론트엔드 구조

```text
frontend/
├── public/
├── src/
│   ├── components/              화면 및 상호작용 컴포넌트
│   ├── features/branchGraph/    그래프 상태, API 어댑터, Mock API
│   ├── lib/                     공통 API 클라이언트
│   ├── App.jsx                  서비스 상태와 주요 사용자 흐름
│   └── main.jsx                 React 진입점
├── package.json
└── vite.config.js
```

## 한계 및 향후 개선

### 현재 한계

- 실제 사용자 환경에서 브랜치 구조가 인지적 부하와 토큰 사용량을 얼마나 줄이는지 추가 검증이 필요하다.
- LLM 응답을 생성 완료 후 표시하고 있어 긴 답변의 체감 대기 시간이 길 수 있다.
- 브랜치가 매우 많아질 경우 그래프 탐색성과 렌더링 성능을 추가로 개선해야 한다.
- 백엔드와 프론트엔드 저장소가 분리되어 있어 API 변경 시 두 저장소의 계약을 함께 관리해야 한다.

### 향후 개선

- LLM 응답 스트리밍을 추가한다.
- 전체 대화뿐 아니라 특정 브랜치 또는 노드만 공유하는 기능을 제공한다.
- 대화 흐름을 분석해 다음 탐색 방향과 병합 대상을 추천한다.
- 웹 검색을 결합해 최신 정보를 대화 컨텍스트에 반영한다.
- 사용자 테스트를 통해 탐색 시간, 브랜치 사용률과 토큰 절감 효과를 실측한다.
