# 프로젝트: LLM Wiki 자동화 시스템

## 아키텍처
본 프로젝트는 로컬 환경에서 Obsidian과 연동 가능한 자동화된 LLM Wiki를 구현합니다. `raw/` 폴더에서 원천 문서(멀티페이지 텍스트와 이미지를 포함한 PDF 파서 출력물 또는 단일 텍스트/MD 파일)를 수집하고, 에이전트가 직접 문서를 분석하여 `wiki/` 폴더 내에 지식 카드, 엔티티, 개념, 인덱스 및 업데이트 로그가 구조화된 위키를 빌드 및 최신화합니다.

### 디렉토리 구조
```
llm-wiki/
├── raw/                         # 원천 문서 보관소 (입력)
│   ├── {doc_id}/                # PDF 파서 출력 디렉토리
│   │   ├── pages/               # 페이지별 텍스트 파일 (예: page_1.txt)
│   │   └── images/              # 추출된 이미지 자산
│   └── {doc_id}.md              # 단일 MD/TXT 원천 문서
├── wiki/                        # 에이전트가 관리하는 Obsidian 위키 (출력)
│   ├── contents/                 # 각 문서별 개념/개체 상세 지식 카드 ({concept}_{doc_id}.md 등)
│   ├── entities/                # 추출 및 연결된 엔티티(개체) 허브 페이지
│   ├── concepts/                # 추출 및 연결된 개념 허브 페이지
│   ├── index.md                 # 메인 위키 인덱스
│   └── log.md                   # 위키 업데이트 로그
├── AGENTS.md                    # 에이전트 mesh 구조 SOP 행동 가이드라인
├── PROJECT.md                   # 프로젝트 스펙 및 인터페이스 계약서 (본 파일)
├── README.md                    # 프로젝트 루트 사용자 설명서 및 아키텍처 가이드
└── skills/
    └── llm-wiki-management/
        └── SKILL.md             # 에이전트용 위키 유지 관리 SOP 매뉴얼
```

## 인터페이스 규약 (Interface Contracts)
- **에이전트 기반 분석 (Agent-driven SOP)**:
  - `raw/`에 신규 문서 등록 시, 에이전트가 직접 위키망(Mesh) 구조를 갱신합니다.
- **이미지 토큰 치환**: `[EMBED:IMAGE:{uuid}:{caption}]`
  - `![caption](../raw/{doc_id}/images/{uuid}.png)` (또는 적절한 상대 경로)로 치환됩니다.
- **출처 참조 방식**:
  - 상세 지식 카드에서 다른 개념/개체를 `[[concepts/개념]]` 또는 `[[entities/개체]]` 형태로 연결합니다.
  - 허브 페이지에서 소스 지식 카드로 `[[contents/{concept}_{doc_id}]]` 또는 `[[contents/{entity}_{doc_id}]]` 형태로 연결합니다.

## 마일스톤 (Milestones)
| 번호 | 이름 | 범위 | 의존성 | 상태 | 대화 ID (Conversation ID) |
|---|---|---|---|---|---|
| 1 | E2E 테스트 트랙 | 테스트 인프라 정의, E2E 테스트 작성 (Tiers 1-4), `TEST_READY.md` 생성 | 없음 | 완료 (DONE) | b8df18b4-0e4e-4d6e-af92-831725a78fdb |
| 2 | 프로토타입 구축 | `raw/` 샘플 데이터 초기화 및 `wiki/` 디렉토리 스켈레톤 생성 | 없음 | 완료 (DONE) | 5dd6650d-32f8-42ae-994b-27f4e49fdade |
| 3 | 에이전트 mesh 구조 SOP | 에이전트 주도의 위키망(Mesh) 분석 및 업데이트 SOP 정의 | M2 | 완료 (DONE) | 8247db4e-2539-4b19-b108-560ad70b62eb |
| 4 | 에이전트 스킬 작성 | `skills/llm-wiki-management/SKILL.md` 에 에이전트 작동 가이드라인 생성 | 없음 | 완료 (DONE) | 1eb33b8c-3c14-439c-8f19-ac53f20c7c1e |
| 5 | 통합 및 검증 | mesh 구조 SOP 검증, 포렌식 감사 통과, README.md 작성 | M1, M3, M4 | 완료 (DONE) | 13e87e60-6480-4023-bae6-0cb8d76379ab |

## 코드 레이아웃 (Code Layout)
- `wiki/`: 에이전트가 관리하는 출력 Vault.
- `.agents/skills/`: 에이전트 실행에 필요한 스킬 매뉴얼 폴더.
