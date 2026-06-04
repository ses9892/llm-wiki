# E2E Test Infra: LLM Wiki Automation System

## Test Philosophy
- Verify compliance with the Agent-driven Mesh SOP.
- Methodology: Structural auditing of the Obsidian wiki vault, verifying markdown link integrity, alphabetical sorting, and log updates.

## Feature Inventory
| # | Feature | Validation Method |
|---|---------|-------------------|
| 1 | Agent-driven wiki updates | Check that additions to `raw/` trigger direct agent updates to `wiki/` |
| 2 | Detailed Knowledge Cards creation | Confirm cards are generated under `wiki/contents/{concept/entity}_{doc_id}.md` |
| 3 | Hub Pages update | Verify hub pages (`wiki/concepts/` and `wiki/entities/`) are updated with 1-2 sentence descriptions and alphabetical contents |
| 4 | Index and Log updates | Check `wiki/index.md` and `wiki/log.md` formatting and content |

## Test Architecture
- Since `compile.py` is removed, automated testing is performed through structural audits of the generated wiki files.
- Checks verify:
  1. No compilation python scripts remain.
  2. The generated Obsidian Vault follows correct directory layout guidelines.
  3. Knowledge card links and hub page connections are consistent.
