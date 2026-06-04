# Implementation Plan - LLM Wiki Query SOP

## Objective
Implement and verify a query protocol (Query SOP) for the local LLM wiki by updating AGENTS.md and SKILL.md, and verifying via a simulated query.

## Planned Steps

### 1. Codebase Investigation
- Already performed. Identified `AGENTS.md` at the root and `SKILL.md` at `.agents/SKILLS/llm-wiki-management/SKILL.md`.
- Confirmed the lack of `TestConcept` and `AnotherConcept` files, which need to be simulated or set up for verification.

### 2. Update AGENTS.md (via Worker Subagent)
- Update `AGENTS.md` to add "4. 위키 질의 해결 원칙 (Query Rules)".
- Define the path-based traversal (`index.md` -> hub -> `contents/`) and mandate citations at the bottom.

### 3. Update SKILL.md (via Worker Subagent)
- Update `.agents/SKILLS/llm-wiki-management/SKILL.md` to add "4. 질의 조회 규칙 (Query Operations Protocol)".
- Detailed 5-step process (Index Matching, Hub Definition Reading, Target Content Card Reading, Mesh Traversal, Answer Formulation & Citation) with examples and markdown format.

### 4. Create Simulated Test Data (via Worker Subagent)
- To perform the verification step, we need `wiki/contents/TestConcept_re_test_doc.md` and `wiki/contents/AnotherConcept_re_test_doc.md` to exist, along with their hub pages `wiki/concepts/TestConcept.md` and `wiki/concepts/AnotherConcept.md` and being registered in `wiki/index.md`.
- This ensures the verifying agent can actually traverse the wiki according to the SOP and find the logical causality.

### 5. Verify via Query Simulation (via Reviewer/Challenger/Critic Subagent)
- Query the subagent: "TestConcept과 AnotherConcept의 논리적 인과관계를 위키 출처를 인용하여 구체적으로 설명해 줘."
- Validate that it follows the Query SOP and outputs correct citations matching `[[contents/TestConcept_re_test_doc]]` and `[[contents/AnotherConcept_re_test_doc]]`.

## Verification Criteria
- `AGENTS.md` contains the new section "4. 위키 질의 해결 원칙 (Query Rules)".
- `SKILL.md` contains the new section "4. 질의 조회 규칙 (Query Operations Protocol)".
- Simulation query output matches the required citations.
