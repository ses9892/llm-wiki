---
name: llm-wiki-management
description: Obsidian 호환 로컬 LLM 위키를 관리하기 위한 행동 가이드라인 및 절차서입니다. 에이전트 주도로 raw/ 소스 문서를 직접 분석하여 상세 지식 카드를 작성하고 개념/개체 허브를 업데이트하는 mesh 구조 SOP를 설명합니다.
---

# LLM Wiki 관리 지침서 (SKILL)

이 지침서는 로컬 LLM 위키를 구성, 관리 및 유지하는 데 필요한 모든 지시사항을 제공합니다. 파이썬 컴파일 스크립트 대신, 에이전트가 직접 원천 문서를 분석하고 망형(Mesh) 연결 관계를 구축하여 위키를 업데이트하는 에이전트 기반 mesh 구조 SOP를 설명하며, 임무를 맡은 모든 에이전트와 시스템이 준수해야 할 핵심 가이드라인 역할을 수행합니다.

---

## 1. 디렉토리 구조 (Directory Structure)

LLM 위키는 가공되지 않은 원천 데이터(입력)와 구조화된 위키 데이터(출력)를 철저히 분리하여 관리합니다.

### 1.1 원천 데이터 보관소 (`raw/`)
`raw/` 폴더는 위키 생성의 대상이 되는 원본 문서들이 위치하는 읽기 전용 디렉토리입니다.
- **PDF 파서 출력 폴더 (`raw/{doc_id}/`)**:
  - `raw/{doc_id}/pages/`: 페이지별 텍스트 파일들 (예: `page_1.txt`, `page_2.txt` 등).
  - `raw/{doc_id}/images/`: 고유 식별자(UUID)로 명명된 추출 이미지 자산들 (예: `uuid123.png`, `pic.jpg`).
- **단일 소스 파일 (`raw/{doc_id}.md` 또는 `raw/{doc_id}.txt`)**:
  - 문서 전체 내용을 담고 있는 단일 마크다운 또는 일반 텍스트 파일입니다.
- **단일 소스용 공용 이미지 폴더 (`raw/images/`)**:
  - 단일 소스 마크다운 파일 내에서 참조되는 이미지 파일들이 보관되는 공용 공간입니다.

### 1.2 컴파일된 위키 디렉토리 (`wiki/` 또는 커스텀 `wiki_dir`)
에이전트에 의해 도출된 출력물들은 Obsidian 뷰어에서 즉시 사용할 수 있도록 다음과 같이 정돈됩니다.
- `wiki/contents/`: 각 원천 문서(`doc_id`)에서 추출된 개념/개체별 상세 지식 카드가 생성되는 폴더. 파일명 포맷: `{concept}_{doc_id}.md` 또는 `{entity}_{doc_id}.md`.
- `wiki/entities/`: 추출된 개체(인물, 조직, 제품 등)의 정의와 관련 지식 카드들의 연결 링크를 기재하는 허브 페이지 폴더 (`{entity}.md`).
- `wiki/concepts/`: 추출된 개념(기술, 알고리즘, 수학적 정의 등)의 정의와 관련 지식 카드들의 연결 링크를 기재하는 허브 페이지 폴더 (`{concept}.md`).
- `wiki/index.md`: 전체 개념 및 지식 카드 콘텐츠 목록이 알파벳순으로 정렬되어 자동 업데이트되는 메인 목차이자 게이트웨이 파일입니다.
- `wiki/log.md`: 타임스탬프, 문서 ID, 상태, 세부 업데이트 내용이 누적되는 히스토리 데이터 테이블입니다.

---

## 2. 에이전트 기반 mesh 구조 SOP (Standard Operating Procedure)

에이전트는 원천 문서가 추가/수정될 때 직접 위키 콘텐츠와 구조를 갱신합니다.

### 2.1 상세 지식 카드 작성 규칙 (`wiki/contents/`)
1. **문서 맥락 분석**: 에이전트는 원천 문서(`raw/` 내 파일)를 읽고 중요하게 다루어지는 개념(Concept)과 개체(Entity)를 식별합니다.
2. **상세 지식 카드 생성**: 식별된 개념/개체마다 `wiki/contents/{concept}_{doc_id}.md` 또는 `wiki/contents/{entity}_{doc_id}.md` 카드를 생성합니다.
3. **내용 기술**: 카드 내부에 해당 문서에서 그 개념/개체가 등장한 구체적인 내용과 이론적 맥락을 기술합니다.
4. **상호 연결 (Mesh)**: 카드 본문에서 다른 개념 및 개체를 언급할 때는 추상 허브 페이지(`[[concepts/다른개념]]` 또는 `[[entities/다른개체]]`)로 링크해서는 안 됩니다. 대신, 반드시 동일 문서(`doc_id`) 내의 구체적인 지식 카드 링크 `[[contents/다른개념_{doc_id}]]` 또는 `[[contents/다른개체_{doc_id}]]` 형태로 상호 직접 연결(Mesh)하여 유기적인 망형 구조를 형성합니다.

### 2.2 허브 페이지 작성 및 갱신 규칙 (`wiki/concepts/` 및 `wiki/entities/`)
1. **정의 작성**: 개념/개체별 허브 페이지(`wiki/concepts/{concept}.md` 또는 `wiki/entities/{entity}.md`) 상단에, 소스 문맥에 기반한 1~2문장 내외의 정교하고 간결한 정의 및 설명(Description)을 작성합니다.
2. **콘텐츠 연결**: `## Contents` 헤더 아래에, 해당 개념/개체를 다룬 문서별 상세 지식 카드 링크(`[[contents/{concept}_{doc_id}]]` 혹은 `[[contents/{entity}_{doc_id}]]`)들을 **알파벳순(Alphabetical Order)**으로 나열합니다.

### 2.3 인덱스 및 로그 동기화 규칙
1. **메인 인덱스 (`wiki/index.md`)**:
   - `## Concepts` 섹션에 새로 발굴된 개념 허브 링크들을 알파벳순으로 추가합니다.
   - `## Contents` 섹션에 새로 생성된 상세 지식 카드 링크들을 알파벳순으로 추가합니다.
2. **로그 테이블 (`wiki/log.md`)**:
   - 업데이트가 완료되면 타임스탬프, 문서 ID, 상태(Updated), 작업 요약(Notes)을 로그 테이블에 한 줄 추가합니다.

---

## 3. 마크다운 포맷팅 및 이미지 처리 준수 사항

### 3.1 Obsidian 위키링크
- 에이전트는 위키 내 문서 간 연결 시 더블 대괄호 `[[...]]` 형식을 철저히 사용합니다.
- 허브 페이지 링크는 `[[concepts/개념명]]` 또는 `[[entities/개체명]]` 형태로 연결합니다.
- 상세 지식 카드 간의 상호 직접 연결은 `[[contents/개념명_문서ID]]` 형태로 연결합니다.
### 3.2 이미지 임베딩 규칙
- 원천 문서 내의 이미지 토큰 `[EMBED:IMAGE:{uuid}:{caption}]`을 탐지하면 마크다운 표준 이미지 링크 포맷으로 치환하여 지식 카드 본문에 포함시킵니다.
- 치환 포맷:
  - PDF 디렉토리 소스: `![caption](../../raw/{doc_id}/images/{uuid}.png)`
  - 단일 소스 파일: `![caption](../../raw/images/{uuid}.png)`
- 실제 이미지 파일의 존재 및 확장자(`.png`, `.jpg`, `.jpeg`, `.gif`, `.webp`)를 확인하여 알맞은 파일명을 지정합니다.

---

## 4. 질의 조회 규칙 (Query Operations Protocol)

에이전트가 위키에 축적된 그물망형 지식 조각들을 정확하게 탐색하고 인용 출처가 달린 최선의 답변을 조립해 내기 위해 다음의 5단계 질의 조회 절차(Query SOP)를 준수하여 정보를 탐색하고 조합해야 합니다.

### 4.1 5단계 질의 조회 절차
1. **인덱스 매칭**: 질문 내 명사/용어를 `wiki/index.md` 개념/개체 목록과 대조하여 관련된 허브 페이지들을 식별합니다.
2. **허브 정의 독해**: 매칭된 개념/개체 허브 페이지(예: `wiki/concepts/{concept}.md` 또는 `wiki/entities/{entity}.md`)를 조회하여 정의 및 설명(Description)을 읽고 문맥의 기초를 파악합니다.
3. **지식 카드 집중 타겟팅**: 허브 페이지 아래 `## Contents` 섹션에 연결된 문서별 상세 지식 카드(예: `wiki/contents/{concept}_{doc_id}.md` 또는 `wiki/contents/{entity}_{doc_id}.md`)들 중 질문과 관련이 깊은 특정 카드를 탐색하여 집중적으로 독해합니다.
4. **다이렉트 링크 망형 추적 (Mesh Traversal)**: 상세 지식 카드 본문 내에 적힌 다이렉트 위키링크(예: `[[contents/다른개념_{doc_id}]]`)를 역추적하여 관련 개념/개체 간의 인과관계나 논리적 흐름을 연쇄적으로 추적하고 수집합니다.
5. **답변 구성 및 인용 기입**: 수집된 인과관계와 문맥을 조합하여 사용자 질문에 답변하는 글을 작성하고, 답변의 맨 하단에 근거가 된 지식 카드들의 상대 경로(예: `[[contents/TestConcept_re_test_doc]]`)를 누락 없이 정확하게 인용(Citations)으로 표기합니다.

### 4.2 예시 및 서식
**질문**: `TestConcept과 AnotherConcept의 논리적 인과관계를 위키 출처를 인용하여 구체적으로 설명해 줘.`

**조회 흐름 예시**:
1. `wiki/index.md`를 열어 `TestConcept`과 `AnotherConcept`이 각각 `wiki/concepts/TestConcept.md`, `wiki/concepts/AnotherConcept.md` 허브로 존재하는 것을 확인.
2. 각 허브의 정의 및 `## Contents` 섹션을 읽어 관련된 상세 지식 카드인 `wiki/contents/TestConcept_re_test_doc.md`와 `wiki/contents/AnotherConcept_re_test_doc.md`를 식별.
3. `wiki/contents/TestConcept_re_test_doc.md`를 독해하는 중, 본문에 포함된 `[[contents/AnotherConcept_re_test_doc]]` 다이렉트 링크를 발견하고 이를 추적하여 두 개념의 직접적인 인과관계를 파악.
4. 두 지식 카드의 내용을 바탕으로 논리적인 인과관계를 구조화하여 작성하고, 하단에 인용을 추가.

**출력 서식 예시**:
```markdown
TestConcept은 ~~~한 원인으로 작용하며, 이는 AnotherConcept의 ~~~에 직접적인 영향을 미칩니다. (상세 내용 기술)

**Citations:**
- [[contents/TestConcept_re_test_doc]]
- [[contents/AnotherConcept_re_test_doc]]
```

