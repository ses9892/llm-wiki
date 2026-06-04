# NotebookLM

**NotebookLM**은 구글에서 개발한 사용자의 문서를 기반으로 학습하고 질문에 답하는 AI 노트 작성 및 분석 도구입니다.

[[contents/Andrej Karpathy_karpathy_llm_wiki]]의 Gist에서 NotebookLM은 전통적인 [[contents/RAG_karpathy_llm_wiki]] 메커니즘을 사용하는 대표적인 기존 시스템 예시로 언급됩니다. 사용자가 원천 자료를 모아 업로드하면 쿼리할 때마다 텍스트를 청크로 쪼개 검색해 답을 재유도하므로, 질의 간에 지식이 축적되거나 유기적으로 엮이지 않는 단순 1차원적 조회 방식([[contents/Compilation vs Retrieval_karpathy_llm_wiki]])의 대명사로 비교 설명됩니다.
