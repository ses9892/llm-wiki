# qmd

**qmd**는 로컬 환경의 마크다운 파일 컬렉션을 빠르게 탐색하기 위해 제작된 로컬 검색 엔진 엔진입니다.

BM25 알고리즘과 임베딩 기반 벡터 검색을 적절히 결합한 하이브리드 검색을 제공하며, 최종적으로 LLM을 통한 재정렬(Re-ranking)을 거쳐 정확한 문맥을 도출합니다. CLI 명령어 형식과 MCP(Model Context Protocol) 서버를 모두 지원하여, LLM 에이전트가 [[contents/Obsidian_karpathy_llm_wiki]] 기반의 대규모 위키 지식을 직접 탐색하고 관련 카드를 인출해 갈 수 있는 기계적 브릿지 역할을 수행합니다.
