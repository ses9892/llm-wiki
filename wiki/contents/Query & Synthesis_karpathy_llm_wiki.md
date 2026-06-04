# Query & Synthesis

**Query & Synthesis(질의 및 합성)**는 컴파일된 위키 지식을 기반으로 사용자가 필요한 정보를 묻고 LLM이 이를 논리적으로 융합하여 정교한 답변을 생산해 내는 과정입니다.

LLM은 사용자의 질의가 입력되면 가장 먼저 위키의 메인 인덱스를 참조하여 연관이 깊은 개념 및 개체 카드들을 선별해 읽어 들입니다. 이후 해당 카드들을 종합하여 일차원적인 사실 나열을 넘어선 입체적인 종합 답변(Synthesis)을 도출합니다. 이 과정에서 작성된 비교 분석 테이블이나 [[contents/Marp_karpathy_llm_wiki]]와 같은 슬라이드 포맷 등 가치 있는 산출물은 채팅 창 속의 일회성 대화로 휘발시키지 않고, 위키에 신규 지식 카드로 재등록(File back to the wiki)하여 영구적으로 축적([[contents/Compounding Artifact_karpathy_llm_wiki]])하는 선순환 구조를 만듭니다.
