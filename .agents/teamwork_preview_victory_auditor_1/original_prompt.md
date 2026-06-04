## Victory Auditor Prompt

Verify the implementation of the LLM Wiki Query SOP in `AGENTS.md` and `SKILL.md`.

### Audit Requirements
1. Confirm that `AGENTS.md` contains "4. 위키 질의 해결 원칙 (Query Rules)" which mandates path-based traversal (index ➡️ hub ➡️ contents) and relative path citations.
2. Confirm that `SKILL.md` contains "4. 질의 조회 규칙 (Query Operations Protocol)" outlining the 5-step traversal process with specific markdown formats and examples.
3. Confirm that simulated query execution for `"TestConcept과 AnotherConcept의 논리적 인과관계를 위키 출처를 인용하여 구체적으로 설명해 줘."` produces a response following the Query SOP and accurately citing `[[contents/TestConcept_re_test_doc]]` and `[[contents/AnotherConcept_re_test_doc]]` (or absolute/relative markdown file links) as sources.
4. Output a structured verdict: `VICTORY CONFIRMED` or `VICTORY REJECTED` with a detailed audit report.

## 2026-06-04T09:12:33Z
You are the Victory Auditor. Your task is to perform an independent victory audit on the implementation of the LLM Wiki Query SOP in AGENTS.md and SKILL.md.
Please review the files in the workspace (AGENTS.md, SKILL.md, wiki/) and verify the mock query response.
Your working directory is `/Users/jangjinho/SeedAi/llm-wiki/.agents/teamwork_preview_victory_auditor_1/`.
Refer to `.agents/teamwork_preview_victory_auditor_1/original_prompt.md` for specific details.
Provide a clear verdict: VICTORY CONFIRMED or VICTORY REJECTED with a detailed report.
