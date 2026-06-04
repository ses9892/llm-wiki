# Compilation vs Retrieval

기존의 정보 검색 및 질의응답 시스템(예: [[contents/NotebookLM_karpathy_llm_wiki]])은 사용자가 질문을 던지는 쿼리 시점(Query-time)에 원천 문서에서 관련 텍스트 조각을 검색하여 임시적으로 답을 구성하는 [[contents/RAG_karpathy_llm_wiki]] 패러다임을 주로 따릅니다. 이 방식은 매 질문마다 지식을 새로 탐색하고 재구성하므로 정보의 축적(Accumulation)이나 합성이 일어나지 않는 한계가 있습니다.

이와 대비되는 개념으로 **Compilation vs Retrieval**은 지식을 검색 시점에 임시 방편으로 찾아내는 것이 아니라, 새로운 소스가 추가될 때마다 기존 지식 체계에 통합 및 컴파일하여 상호 연결된 하나의 거대한 영구적 지식 자산([[contents/Compounding Artifact_karpathy_llm_wiki]])으로 미리 빌드해 두는 방식입니다. 이로 인해 동일한 질문이나 복잡한 다중 문서 합성 질문이 들어왔을 때, 지식을 처음부터 다시 유도하는 대신 미리 정제되고 업데이트된 컴파일본을 기반으로 빠르고 정확하게 답할 수 있게 됩니다.
