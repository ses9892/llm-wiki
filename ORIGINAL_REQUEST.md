# Original User Request

## Initial Request — 2026-06-04T11:33:27+09:00

로컬 디렉토리 구조화와 Obsidian 연동을 테스트하기 위해 샘플 문서를 배치하고, PDF 파서 결과물 및 일반 텍스트/MD 파일을 감지하여 위키를 자동 갱신(컴파일)하는 자동화 스크립트와 에이전트용 위키 작성 스킬(지침/도구), 그리고 지식 관리 사용법이 담긴 README.md를 구축합니다.

Working directory: /Users/jangjinho/SeedAi/llm-wiki
Integrity mode: development

## Requirements

### R1. 로컬 프로토타입 샘플 데이터 배치 및 디렉토리 구조화
- 설계안에 따라 `raw/` 하위에 구조화된 PDF 파싱 샘플(Attention paper 예시 등) 및 일반 텍스트/MD 문서를 배치하고, `wiki/` 폴더 하위에 기본 파일(`index.md`, `log.md` 등)을 초기 구조화합니다.

### R2. LLM Wiki 자동 컴파일러 스크립트 개발
- 언어: Python
- 동작 모드: CLI 명령어 모드(예: `python compile.py raw/{doc_id}`) 및 파일 변경 실시간 감시 모드(Watcher) 지원.
- 기능: `raw/`에 새로 배치되는 입력(PDF 파싱 폴더 또는 단일 TXT/MD 파일)을 감지하고, `CLAUDE.md` 규칙을 파싱하여 위키(`wiki/sources/`, `wiki/entities/`, `wiki/concepts/`, `wiki/index.md`, `wiki/log.md`)를 갱신/컴파일합니다.

### R3. 에이전트용 위키 작성 스킬(Skill) 설계
- 외부 에이전트나 시스템이 위키 문서를 작성/편집할 때 참고하여 바로 사용할 수 있는 스킬 지침(스킬 파일 및 지침)을 구성합니다.

### R4. Wiki 사용 설명서(README.md) 작성
- 루트 디렉토리에 인간 사용자와 에이전트가 이 LLM Wiki 시스템을 어떻게 사용하고 관리하는지 설명하는 고품질 README.md를 작성합니다.

## Acceptance Criteria

### 구조 및 컴파일러 검증
- [ ] `raw/` 하위에 PDF 파싱 샘플(Attention Paper 구조)을 배치하고 `compile.py` 실행 시 오류 없이 완료되어야 합니다.
- [ ] `raw/` 하위에 단일 `.txt` 또는 `.md` 파일을 배치하고 실행 시 해당 문서의 `wiki/sources/summary_*.md`가 정상 생성되어야 합니다.
- [ ] `raw/` 하위 파일 수정 시 Watcher 모드에서 변경사항을 감지하여 `wiki/` 문서들이 실시간 컴파일되어야 합니다.
- [ ] `[EMBED:IMAGE:{uuid}:{caption}]` 토큰이 적절한 로컬 마크다운 이미지 경로(`![caption](...)`)로 자동 변환되어 저장되어야 합니다.
- [ ] 단일 텍스트/MD 파일의 출처는 `pages` 디렉토리가 없으므로 파일 링크(`[[raw/{doc_id}.md]]`) 혹은 헤더 앵커를 참조하게 설정되어야 합니다.

### 스킬 및 문서화 검증
- [ ] 에이전트가 위키 관리를 위한 규칙을 숙지할 수 있도록 돕는 에이전트 스킬 지침서 파일이 지정 경로에 생성되어야 합니다.
- [ ] 루트에 시스템 아키텍처, 작동 방법, Obsidian 연동 팁 등이 담긴 `README.md`가 생성되어야 합니다.

## Follow-up — 2026-06-04T16:54:08+09:00

로컬 LLM Wiki 시스템을 파이썬 스크립트 기반의 기계적 빌드 방식에서 에이전트(LLM)가 직접 문맥을 분석하여 유기적으로 지식을 엮는 LLM 주도형 그물망 위키 구조로 개편합니다. 기존 `compile.py` 스크립트는 완전히 제거하며, 모든 위키 가공 및 동기화 작업은 에이전트가 직접 처리합니다.

Working directory: /Users/jangjinho/SeedAi/llm-wiki
Integrity mode: development

## Requirements

### R1. compile.py 완전 제거
- 기존 `compile.py` 파일을 작업 공간에서 영구 삭제합니다.
- `README.md`, `PROJECT.md` 등 프로젝트 관련 모든 문서에서 `compile.py` 실행 명령어 및 CLI 옵션에 관한 언급을 제거하거나 수정합니다.

### R2. 에이전트 주도 위키 관리 지침서(SOP) 구축 및 실행
- 전역 규칙 AGENTS.md와 에이전트 스킬 SKILL.md를 전면 수정합니다.
- 새로운 소스 문서(`raw/`)가 추가되었을 때, 에이전트가 직접 분석하여 다음과 같은 구조로 위키를 구성하고 동기화하도록 지침을 설계합니다.
  1. **개념/개체 허브 페이지 (`wiki/concepts/{concept}.md` 등)**: 이 개념에 대한 간략한 정의와, 이 개념을 자세히 설명하고 있는 지식 카드들의 링크 목록(`[[sources/{concept}_{doc_id}]]`)을 알파벳 순으로 정렬하여 기입합니다.
  2. **상세 지식 카드 페이지 (`wiki/sources/{concept}_{doc_id}.md` 등)**: 해당 개념이 `doc_id` 문서에서 어떻게 설명되었는지 상세 맥락을 서술하며, 서술 내에 관련된 다른 개념/개체를 위키링크(`[[concepts/다른개념]]`)로 삽입하여 양방향 연결되도록 엮습니다.
  3. **메인 인덱스 및 로그 동기화 (`wiki/index.md`, `wiki/log.md`)**: 신규 추가된 개념과 지식 조각들을 알파벳 순으로 인덱스 파일에 링크하고 로그 테이블에 기록을 추가합니다.

## Acceptance Criteria

### 구조 및 지침 개편
- [ ] 작업 공간 내에 `compile.py` 파일이 존재하지 않아야 합니다.
- [ ] README.md 및 PROJECT.md 내에 `compile.py` 사용법과 관련된 구시대적 명령어 안내가 모두 제거되어야 합니다.
- [ ] AGENTS.md와 SKILL.md에 LLM 주도 2번 위키 구조(출처별 지식 카드) 생성 및 양방향 엮기 절차가 논리정연하게 명문화되어 있어야 합니다.

### 위키 컴파일 및 동작 검증 (수동 인입 테스트)
- [ ] 에이전트가 가이드라인에 따라 직접 새로운 원본 문서(`raw/test_doc.md`)를 파싱하여 아래의 신규 지식 카드를 올바르게 작성해야 합니다.
  - 예: `wiki/sources/TestConcept_test_doc.md` 파일 생성 및 상세 서술 기입.
- [ ] 생성된 지식 카드 내에서 다른 개념 `[[concepts/AnotherConcept]]` 링크가 존재해야 합니다.
- [ ] `wiki/concepts/TestConcept.md` 허브 파일에 `[[sources/TestConcept_test_doc]]` 링크가 올바르게 엮여 있어야 합니다.
- [ ] `wiki/index.md` 파일 내의 `## Concepts` 및 `## Sources` 섹션에 신규 생성된 파일들이 가나다순으로 등록되어야 합니다.

## Follow-up — 2026-06-04T17:31:04+09:00

로컬 LLM Wiki 시스템에서 지식 조각들의 저장소 명칭을 `sources`에서 `contents`로 전면 변경하고, 지식 조각 간에 추상적 개념 허브를 거치지 않고 직접 엮는 다이렉트 연결 아키텍처로 개편합니다.

Working directory: /Users/jangjinho/SeedAi/llm-wiki
Integrity mode: development

## Requirements

### R1. 디렉토리 명칭 변경 및 일괄 리팩토링
- `wiki/sources/` 폴더명을 `wiki/contents/`로 물리적으로 변경합니다.
- `README.md`, `PROJECT.md`, `TEST_INFRA.md`, `TEST_READY.md` 및 `wiki/index.md`, `wiki/log.md`, `wiki/concepts/*`, `wiki/entities/*` 등 프로젝트 내 모든 마크다운 파일에 기재된 `sources/` 경로 참조와 텍스트를 `contents/`로 일괄 치환(리팩토링)합니다.

### R2. 다이렉트 지식 조각 연결형 SOP 적용 및 지침서 개정
- AGENTS.md 및 SKILL.md를 수정하여 다음의 규칙을 탑재합니다.
  - 지식 카드(`wiki/contents/{concept}_{doc_id}.md`) 본문 텍스트 내에서 다른 용어를 언급할 때, 개념 페이지(`[[concepts/다른용어]]`)가 아닌 그 용어를 실제로 설명하는 다른 콘텐츠 조각 파일(예: `[[contents/다른용어_{doc_id}]]` 혹은 타당한 다른 문서의 지식 카드)을 본문 중에 **직접 위키링크로 지정하여 엮는 절차(SOP)**를 작성합니다.
- 기존에 작성된 `wiki/contents/TestConcept_test_doc.md` 등 기존 지식 조각의 내용 속에 포함되어 있는 concepts 링크도 이 새로운 다이렉트 연결 규칙에 맞추어 `[[contents/...]]` 지식 카드를 직접 가리키도록 일괄 마이그레이션(수정)합니다.

## Acceptance Criteria

### 디렉토리 리팩토링 검증
- [ ] 작업 공간 내에 `wiki/sources/` 폴더가 더 이상 존재하지 않고, `wiki/contents/` 폴더가 존재해야 합니다.
- [ ] 모든 위키 파일 및 설명 문서(`README.md`, `PROJECT.md` 등) 내에 `sources/` 대신 `contents/`로 올바르게 경로가 치환되어 있어야 합니다.

### 지침서 및 기존 링크 마이그레이션 검증
- [ ] AGENTS.md와 SKILL.md에 다이렉트 콘텐츠 링크 연결 규칙이 상세히 명문화되어 있어야 합니다.
- [ ] 기존 생성되었던 모든 지식 카드 내의 `[[concepts/다른개념]]` 형태의 링크들이 실제 콘텐츠 카드 `[[contents/다른개념_{doc_id}]]`를 직접 조준하도록 수정되어야 합니다.

### 동작 검증 (수동 re_test_doc 인입 테스트)
- [ ] 에이전트가 가이드라인에 따라 새로운 원본 문서(`raw/re_test_doc.md`)를 파싱하여 아래 of 콘텐츠 지식 카드를 올바르게 작성해야 합니다.
  - 예: `wiki/contents/TestConcept_re_test_doc.md` 생성 및 상세 서술 기입.
- [ ] 생성된 지식 카드 내에서 다른 개념을 언급할 때 `[[contents/AnotherConcept_re_test_doc]]`와 같이 다이렉트 콘텐츠 카드를 본문 내에 직접 링크해야 합니다.
- [ ] `wiki/concepts/TestConcept.md` 허브 파일에 `[[contents/TestConcept_re_test_doc]]`가 올바르게 기입되어야 합니다.
- [ ] `wiki/index.md` 파일 내에 `sources` 대신 `contents` 섹션으로 올바르게 관리되고 새로운 지식 카드들이 가나다순으로 자동 기재되어야 합니다.

## Follow-up — 2026-06-04T18:07:59+09:00

에이전트가 위키에 축적된 그물망형 지식 조각들을 정확하게 탐색하고 인용 출처가 달린 최선의 답변을 조립해 내도록 하는 위키 질의 조회 프로토콜(Query SOP)을 설계하고 지침서에 탑재합니다.

Working directory: /Users/jangjinho/SeedAi/llm-wiki
Integrity mode: development

## Requirements

### R1. AGENTS.md (전역 규칙)에 질의 해결 원칙 추가
- AGENTS.md 파일에 **"4. 위키 질의 해결 원칙 (Query Rules)"** 섹션을 신규 기재합니다.
- 에이전트가 사용자 질의를 마주했을 때, 임의로 답변을 작성하지 않고 반드시 `index.md` ➡️ 개념/개체 허브 ➡️ 상세 지식 카드(`contents/`) 경로를 추적하여 답변 근거를 확보하도록 규정합니다.
- 최종 답변 하단에 근거가 된 지식 카드들의 상대 파일 경로 링크(인용)를 의무적으로 기입하도록 강제합니다.

### R2. SKILL.md (에이전트 스킬)에 질의 조회 프로토콜 상세화
- SKILL.md 파일 하단에 **"4. 질의 조회 규칙 (Query Operations Protocol)"** 섹션을 추가합니다.
- 에이전트가 아래의 단계를 거쳐 정보를 탐색 및 조합하는 절차를 예시와 마크다운 서식을 곁들여 상세히 가이드합니다.
  1. **인덱스 매칭**: 질문 내 명사/용어를 `index.md` 개념 목록과 대조.
  2. **허브 정의 독해**: 매칭된 개념/개체 허브에서 정의 파악.
  3. **지식 카드 집중 타겟팅**: 허브 아래 Sources 섹션의 특정 카드만 집중 독해하여 문맥 파악.
  4. **다이렉트 링크 망형 추적 (Mesh Traversal)**: 지식 카드 내에 적힌 다이렉트 위키링크(`[[contents/다른개념_{doc_id}]]`)를 역추적하여 관련 인과관계를 연쇄 수집.
  5. **답변 구성 및 인용 기입**: 답변 하단에 근거 카드 파일 경로 표기.

## Acceptance Criteria

### 지침서 개정 검증
- [ ] AGENTS.md 내에 4번 위키 질의 해결 원칙이 논리적으로 명문화되어 있어야 합니다.
- [ ] SKILL.md 내에 4번 질의 조회 프로토콜(Query SOP)의 5단계 흐름과 구체적 예시 및 서식이 완벽하게 기재되어 있어야 합니다.

### 동작 검증 (수동 질문 검증)
- [ ] 에이전트에게 "TestConcept과 AnotherConcept의 논리적 인과관계를 위키 출처를 인용하여 구체적으로 설명해 줘." 라는 가상 질문을 던졌을 때, 수립된 Query SOP를 성실하게 이행하여 답변을 출력해야 합니다.
- [ ] 출력된 답변 하단에 실제 위키 폴더 구조 내의 지식 카드 경로인 `[[contents/TestConcept_re_test_doc]]` 및 `[[contents/AnotherConcept_re_test_doc]]` (또는 file://로 시작하는 마크다운 링크 형식)가 답변의 근거로 누락 없이 정확하게 인용 표기되어야 합니다.
