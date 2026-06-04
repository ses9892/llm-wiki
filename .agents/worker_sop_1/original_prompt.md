## 2026-06-04T18:09:30+09:00

You are a worker agent assigned to update the LLM Wiki guidelines and set up test data.
Your working directory is `/Users/jangjinho/SeedAi/llm-wiki/.agents/worker_sop_1/`.

Tasks:
1. Update `/Users/jangjinho/SeedAi/llm-wiki/AGENTS.md` to add a new section "4. 위키 질의 해결 원칙 (Query Rules)" at the end:
```markdown
---

## 4. 위키 질의 해결 원칙 (Query Rules)

- **경로 기반 탐색**: 에이전트가 사용자 질의를 받았을 때, 임의의 지식이나 추측으로 답변을 작성해서는 안 되며, 반드시 `wiki/index.md` ➡️ 개념/개체 허브 페이지 ➡️ 상세 지식 카드(`wiki/contents/`)의 경로를 추적하여 검증된 답변 근거를 확보해야 합니다.
- **출처 인용 의무화**: 최종 답변 하단에는 답변 구성에 근거가 된 지식 카드들의 상대 파일 경로 링크(예: `[[contents/개념명_문서ID]]`)를 인용(Citations) 섹션으로 의무적으로 기입해야 합니다.
```

2. Update `/Users/jangjinho/SeedAi/llm-wiki/.agents/SKILLS/llm-wiki-management/SKILL.md` to add a new section "4. 질의 조회 규칙 (Query Operations Protocol)" at the end:
```markdown
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
```

3. Create the simulated wiki test files to enable verification of the query:
- Create `wiki/contents/TestConcept_re_test_doc.md` with:
```markdown
# TestConcept_re_test_doc

TestConcept은 LLM 위키 검증용 테스트 개념입니다. TestConcept은 특정한 연산 부하를 발생시키는 주요 요인으로 작용하며, 이는 결국 [[contents/AnotherConcept_re_test_doc]]의 비정상적인 활성화를 초래하게 됩니다.
```
- Create `wiki/contents/AnotherConcept_re_test_doc.md` with:
```markdown
# AnotherConcept_re_test_doc

AnotherConcept은 LLM 위키 검증용 테스트 개념입니다. AnotherConcept은 [[contents/TestConcept_re_test_doc]]로부터 유발된 자극에 의해 활성화되며, 이 두 개념 간의 강한 상호작용은 시스템의 전반적인 동기화 성능에 직접적인 기여를 합니다.
```
- Create `wiki/concepts/TestConcept.md` with:
```markdown
# TestConcept

TestConcept은 검증을 위한 가상 개념입니다.

## Contents
- [[contents/TestConcept_re_test_doc]]
```
- Create `wiki/concepts/AnotherConcept.md` with:
```markdown
# AnotherConcept

AnotherConcept은 검증을 위한 가상 개념입니다.

## Contents
- [[contents/AnotherConcept_re_test_doc]]
```

4. Update `wiki/index.md` to index the new concepts and contents in alphabetical order.
- In `## Concepts` section:
  Add `- [[concepts/AnotherConcept]]` and `- [[concepts/TestConcept]]` in correct alphabetical positions.
- In `## Contents` section:
  Add `- [[contents/AnotherConcept_re_test_doc]]` and `- [[contents/TestConcept_re_test_doc]]` in correct alphabetical positions.

5. Update `wiki/log.md` by appending this entry to the table:
`| 2026-06-04T18:09:21+09:00 | re_test_doc | Updated | Extracted 2 concepts (TestConcept, AnotherConcept). Created 2 detailed knowledge cards and 2 hub pages. |`
