# Local LLM Wiki 자동화 시스템 (Local LLM Wiki Automation System)

본 프로젝트는 로컬 환경에 위치한 다양한 형식의 원본 문서(PDF 파서 출력 텍스트/이미지 또는 단일 마크다운/텍스트 파일)를 파싱하여, 지식 정보가 서로 유기적으로 연결된 Obsidian 호환 형태의 로컬 LLM 위키를 자동으로 구축하고 동기화하는 시스템입니다. 본 시스템은 파이썬 컴파일 스크립트 대신, 에이전트가 직접 문서를 분석하여 위키의 망형(Mesh) 구조를 갱신하고 유지관리하는 에이전트 기반 mesh 구조 SOP를 따릅니다.

---

## 1. 시스템 아키텍처 및 디자인 (System Architecture & Design)

이 시스템은 정적 지식 원본데이터에서 의미 있는 정보 단위(개체 및 개념)를 식별 및 연결하여 상호 참조가 가능한 지식 베이스(Knowledge Base)를 생성합니다. 전체적인 아키텍처 구조와 처리 흐름은 다음과 같습니다.

### 1.1 데이터 처리 흐름 (Workflow)

```
[입력 데이터 (raw/)]
       │
       ▼
┌────────────────────────────────────────┐
│     에이전트 분석 및 위키 업데이트       │
│  - 직접 원천 문서 분석                │
│  - 이미지 토큰 변환 ([EMBED:IMAGE:...]) │
│  - 지식 카드 생성 (wiki/contents/)      │
│  - 개념/개체 허브 페이지 갱신          │
└────────────────────────────────────────┘
       │
       ├────────────────────────┐
       ▼                        ▼
[상세 지식 카드 (wiki/contents/)] [개체 및 개념 허브 (wiki/entities/, wiki/concepts/)]
       │                        │
       ├────────────────────────┘
       ▼
[인덱스 및 로그 동기화 (wiki/index.md, wiki/log.md)]
```

1. **문서 입력 및 분석 (Document Ingestion)**:
   - `raw/` 경로에 새로운 원천 문서(PDF 파서 디렉토리 또는 단일 텍스트/마크다운 파일)가 추가되면, 에이전트가 직접 문서를 분석하여 위키를 업데이트합니다.
   - 문서 파일명 또는 디렉토리 이름에서 고유한 문서 ID(`doc_id`)를 추출하여 지식 카드 및 허브 페이지 링크에 사용합니다.

2. **이미지 토큰 처리 및 경로 해결 (Asset Resolution)**:
   - 문서 내부의 `[EMBED:IMAGE:{uuid}:{caption}]` 형태의 특수 토큰을 탐지합니다.
   - 해당 토큰을 마크다운 표준 이미지 링크 포맷(`![caption](../../raw/{doc_id}/images/{uuid}.{ext})` 등)으로 자동 변환하여 위키 페이지에 삽입합니다.

3. **세맨틱 개체(Entity) 및 개념(Concept) 추출 및 지식 카드 생성 (Semantic Extraction & Knowledge Cards)**:
   - 에이전트는 원천 문서에서 개체(Entity)와 개념(Concept)을 추출하여, 각 문서별 상세 지식을 담은 지식 카드(`wiki/contents/{concept}_{doc_id}.md` 또는 `wiki/contents/{entity}_{doc_id}.md`)를 생성합니다.
   - 이 카드는 해당 문서 내에서 해당 개념/개체가 언급되는 구체적인 맥락을 서술하고, 표준 Obsidian 링크(`[[concepts/다른개념]]` 또는 `[[entities/다른개체]]`)를 통해 다른 개념 및 개체와 연결됩니다.

4. **상호 참조 및 허브 페이지 생성 (Cross-Linking & Hub Pages)**:
   - 추출된 개체(Entity)와 개념(Concept)마다 각각 하나의 허브 페이지(`wiki/entities/{entity}.md` 및 `wiki/concepts/{concept}.md`)를 생성하거나 업데이트합니다.
   - 허브 페이지 상단에는 원본 문맥에 근거한 1~2문장의 간결한 정의/설명(Description)이 포함됩니다.
   - 하단의 `## Contents` 섹션에는 해당 개념/개체에 관련된 상세 지식 카드 링크들(`[[contents/{concept}_{doc_id}]]` 또는 `[[contents/{entity}_{doc_id}]]`)이 알파벳순으로 정렬되어 포함됩니다.

5. **인덱스 및 로그 업데이트 (Index & Log Synchronization)**:
   - 위키의 메인 목차이자 MOC인 `wiki/index.md` 내에 새로운 개념 및 소스(지식 카드) 링크를 알파벳순으로 정렬하여 갱신합니다.
   - 위키 업데이트 일시, 대상 ID, 업데이트 상태(Updated/Failed), 추출된 통계 및 세부 메모를 포함하여 `wiki/log.md` 히스토리 테이블에 업데이트 이력을 기록합니다.

---

## 2. 로컬 디렉토리 구조 (Local Directory Structure)

본 프로젝트는 입력(raw/) 데이터와 출력(wiki/) 데이터를 철저히 분리하여 관리합니다.

```text
llm-wiki/
├── raw/                            # 원본 소스 문서 및 에셋 (입력 폴더)
│   ├── {doc_id}/                   # PDF 파서의 출력 결과물 디렉토리
│   │   ├── pages/                  # 페이지별 분할 텍스트 (예: page_1.txt, page_2.txt)
│   │   └── images/                 # 본문 내에서 참조하는 이미지 파일 ({uuid}.png 등)
│   ├── {doc_id}.md                 # 단일 마크다운 문서 파일
│   └── images/                     # 단일 원본 문서용 공용 이미지 폴더
├── wiki/                           # 에이전트가 관리하는 Obsidian 호환 위키 (출력 Vault)
│   ├── contents/                   # 각 문서별 개념/개체 상세 지식 카드 ({concept}_{doc_id}.md 등)
│   ├── entities/                   # 개체(Entity)별 정의 및 지식 카드 목록 허브 페이지
│   ├── concepts/                   # 개념(Concept)별 정의 및 지식 카드 목록 허브 페이지
│   ├── index.md                    # 위키 메인 대문 및 Map of Contents (MOC)
│   └── log.md                      # 위키 업데이트 히스토리 테이블 로그
├── AGENTS.md                       # 에이전트 mesh 구조 SOP 행동 가이드라인
├── PROJECT.md                      # 프로젝트 스펙 및 인터페이스 계약서
├── README.md                       # 프로젝트 종합 가이드북 (본 파일)
└── skills/
    └── llm-wiki-management/
        └── SKILL.md                # 에이전트용 위키 유지 관리 SOP 매뉴얼
```

---

## 3. 에이전트 기반 mesh 구조 SOP (Agent-driven Mesh SOP)

본 시스템은 정해진 스크립트에 의존하지 않고 에이전트가 주도적으로 위키를 분석하고 구조화하는 방식을 취합니다. 

### 3.1 업데이트 트리거 및 절차
1. **신규 문서 감지**: `raw/` 폴더에 새로운 문서가 추가되거나 기존 문서가 수정되면 에이전트 태스크가 활성화됩니다.
2. **의미적 맥락 분석**: 에이전트는 원천 문서에 언급된 주요 개념(Concept)과 개체(Entity)를 식별하고, 해당 문서 안에서의 정의 및 맥락을 파악합니다.
3. **지식 카드 및 허브 갱신**:
   - `wiki/contents/{concept}_{doc_id}.md` 등의 지식 카드를 작성합니다.
   - `wiki/concepts/{concept}.md` 등의 허브 페이지를 개설 또는 업데이트하여 정의를 최신화하고 알파벳순으로 소스 링크를 추가합니다.
4. **인덱스 및 로그 업데이트**: `wiki/index.md`와 `wiki/log.md`를 즉시 갱신합니다.

---

## 4. Obsidian 연동 및 활용 팁 (Obsidian Integration Tips)

본 위키 시스템은 개인 지식 관리 툴인 Obsidian 환경에서 최상의 퍼포먼스를 내도록 완전히 최적화되어 설계되었습니다.

### 4.1 위키 저장소(Vault) 불러오기
Obsidian 앱을 실행한 후, `Open folder as vault` 메뉴를 선택하여 생성된 `wiki/` 폴더를 지정합니다. 이 작업을 거치면 Obsidian 내부에서 폴더 트리가 깔끔하게 재구조화되어 가독성 높은 인터페이스를 제공받을 수 있습니다.

### 4.2 더블 브래킷(`[[...]]`)을 통한 입체 네비게이션
- Obsidian 특유 of `[[concepts/개념이름]]` 이나 `[[entities/개체이름]]` 경로를 활용해 문서 간 이동이 매끄럽게 흐르도록 지원합니다.
- 지식 카드(`wiki/contents/`) 내부에서 다른 관련 개념이나 개체를 더블 브래킷으로 연결함으로써, 지식의 단절 없이 거미줄 같은 링크 네트워크(Mesh)를 형성하여 지식을 학습할 수 있습니다.

### 4.3 index.md 및 log.md 활용
- **`index.md` (홈페이지 및 MOC)**: 이 파일을 Obsidian의 홈 화면 또는 시작 페이지로 활용하십시오. 새로 축적된 소스 목록과 개체, 개념 분류가 실시간으로 일목요연하게 업데이트되므로 전체 지식 지도를 그리기에 최적입니다.
- **`log.md` (이력 추적)**: 위키 정보가 최신화되는 흐름을 모니터링할 때 유용하며, 업데이트가 성공했는지 실시간으로 파악하는 대시보드 역할을 합니다.
