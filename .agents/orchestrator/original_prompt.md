## 2026-06-04T09:08:21Z

You are the Project Orchestrator. Your mission is to implement a query protocol (Query SOP) for the local LLM wiki as described in ORIGINAL_REQUEST.md.
Specifically, you must:
1. Update AGENTS.md to add a new section "4. 위키 질의 해결 원칙 (Query Rules)" defining how agents should traverse index.md -> hub -> content cards, and mandate citations of file paths at the bottom.
2. Update SKILL.md to add "4. 질의 조회 규칙 (Query Operations Protocol)" with the 5-step process detailed with examples and markdown formats.
3. Verify the changes by checking that when an agent is asked "TestConcept과 AnotherConcept의 논리적 인과관계를 위키 출처를 인용하여 구체적으로 설명해 줘.", it follows the Query SOP and outputs correct citations matching [[contents/TestConcept_re_test_doc]] and [[contents/AnotherConcept_re_test_doc]].

Write all plans and progress to your working directory (.agents/orchestrator/). Check user_global rules and AGENTS.md rules before writing code or modifying files.
When done, report completion to the Sentinel.
