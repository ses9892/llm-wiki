# RAG

**RAG(Retrieval-Augmented Generation, 검색 증강 생성)**는 질문이 입력되었을 때 외부 데이터베이스에서 관련 문서를 검색하여 거대 언어 모델의 프롬프트 컨텍스트에 주입해 답변의 품질을 높이는 일반적인 아키텍처입니다.

[[contents/NotebookLM_karpathy_llm_wiki]] 등 대다수의 상용 지식 분석 도구가 이 방식을 택하고 있으나, 카파시는 RAG가 매번 질문할 때마다 관련 텍스트 조각을 독립적으로 찾아내 답을 조립할 뿐, 이전 질의응답에서 획득된 통찰이나 복수의 문서들 사이에 얽힌 고차원적 인과관계 및 모순점을 정비해 누적시키지 못하는 근본적 지식 휘발성을 지니고 있음을 지적하였습니다. 이와 대비되는 대안으로 지식을 미리 유기적으로 엮어 컴파일하고 유지하는 LLM Wiki 패턴([[contents/Compilation vs Retrieval_karpathy_llm_wiki]])이 제안되었습니다.
