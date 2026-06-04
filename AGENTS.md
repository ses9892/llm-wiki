# AGENTS.md

## 에이전트 기반 Mesh 구조 SOP (Standard Operating Procedure)

본 문서는 로컬 LLM 위키를 구성하고 관리하는 에이전트가 준수해야 하는 행동 지침 및 절차서(SOP)입니다. 본 프로젝트는 Python 컴파일 스크립트 대신, 에이전트가 직접 원천 문서를 분석하고 위키의 망형(Mesh) 연결 관계를 구축하는 구조를 취합니다.

---

## 1. 위키 업데이트 기본 원칙

1. **에이전트 직접 분석 및 업데이트 (Agent-driven Updates)**:
   - `raw/` 디렉토리에 새로운 원천 문서가 추가되거나 기존 문서가 수정되면, 에이전트는 별도의 python 컴파일 스크립트를 실행하지 않고 직접 문서를 분석하여 위키를 업데이트합니다.
   
2. **지식 격자 구조 (Mesh Structure)**:
   - 각 문서(`doc_id`)에서 언급된 개념(Concept)과 개체(Entity)는 문서별로 상세 지식 카드(Knowledge Card)로 분리되어 기록됩니다.
   - 개념/개체별로 하나의 허브 페이지(Hub Page)가 존재하여, 개별 문서들에 흩어진 상세 지식 카드들을 연결하는 관문 역할을 합니다.

---

## 2. 상세 지식 카드 및 허브 페이지 생성 규칙

### 2.1 상세 지식 카드 생성 (`wiki/contents/`)
- **생성 대상**: 문서 `doc_id`에서 중요하게 다루어지는 개념(Concept) 또는 개체(Entity)마다 개별 카드를 생성합니다.
- **경로 및 파일명**: `wiki/contents/{concept}_{doc_id}.md` 또는 `wiki/contents/{entity}_{doc_id}.md`
- **카드 내용**:
  - 해당 원천 문서 내에서 해당 개념/개체가 다루어지는 구체적인 맥락(Context)과 내용을 상세히 기술합니다.
  - 본문 내에서 언급되는 다른 관련 개념 및 개체들은 추상 허브 페이지(`[[concepts/다른개념]]` 또는 `[[entities/다른개체]]`)로 링크해서는 안 됩니다. 대신, 반드시 동일 문서(`doc_id`) 내의 구체적인 지식 카드 링크 `[[contents/다른개념_{doc_id}]]` 또는 `[[contents/다른개체_{doc_id}]]` 형태로 상호 직접 연결(Mesh)해야 합니다.
  - 필요한 경우, 원본 이미지 토큰 `[EMBED:IMAGE:{uuid}:{caption}]`을 파싱하여 마크다운 이미지 링크로 변환해 포함시킵니다.

### 2.2 허브 페이지 생성 및 업데이트 (`wiki/concepts/` 및 `wiki/entities/`)
- **생성 대상**: 추출된 개념 및 개체당 하나의 허브 페이지를 개설하거나 기존 페이지를 업데이트합니다.
- **경로 및 파일명**: `wiki/concepts/{concept}.md` 또는 `wiki/entities/{entity}.md`
- **허브 페이지 내용**:
  - **정의 (Definition)**: 최상단에 해당 개념/개체에 대한 원본 문맥에 근거한 1~2문장의 간결한 정의 및 설명(Description)을 기재합니다.
  - **콘텐츠 목록 (Contents)**: `## Contents` 헤더 아래에, 해당 개념/개체를 다루는 상세 지식 카드 링크 리스트(`[[contents/{concept}_{doc_id}]]` 또는 `[[contents/{entity}_{doc_id}]]`)를 **알파벳순(Alphabetical Order)**으로 정렬하여 기입합니다.

---

## 3. 인덱스 및 로그 동기화

### 3.1 메인 인덱스 업데이트 (`wiki/index.md`)
- 에이전트는 새로운 지식 카드와 개념/개체가 생성되면 `wiki/index.md`를 업데이트해야 합니다.
- `## Concepts`, `## Entities`, `## Contents` 헤더 아래에 새로운 항목 링크를 **알파벳순(Alphabetical Order)**으로 추가합니다.

### 3.2 히스토리 로그 업데이트 (`wiki/log.md`)
- 에이전트가 위키를 업데이트할 때마다 `wiki/log.md` 파일에 기록을 누적합니다.
- 테이블 형식으로 다음 정보를 기록합니다:
  - 타임스탬프 (Timestamp)
  - 문서 ID (Document ID)
  - 처리 상태 (Status - 예: "Updated", "Failed")
  - 분석 내용 및 추출된 개념/개체 개수 등 상세 사항 (Notes)

---

## 4. 위키 질의 해결 원칙 (Query Rules)

- **경로 기반 탐색**: 에이전트가 사용자 질의를 받았을 때, 임의의 지식이나 추측으로 답변을 작성해서는 안 되며, 반드시 `wiki/index.md` ➡️ 개념/개체 허브 페이지 ➡️ 상세 지식 카드(`wiki/contents/`)의 경로를 추적하여 검증된 답변 근거를 확보해야 합니다.
- **출처 인용 의무화**: 최종 답변 하단에는 답변 구성에 근거가 된 지식 카드들의 상대 파일 경로 링크(예: `[[contents/개념명_문서ID]]`)를 인용(Citations) 섹션으로 의무적으로 기입해야 합니다.
