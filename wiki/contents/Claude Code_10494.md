# Claude Code (Document 10494)

Claude Code는 Anthropic에서 출시한 터미널 기반 AI 코딩 에이전트로, [[contents/designlang_10494]]과의 직접적인 긴밀한 플러그인 통합 및 연동을 지원한다.

## 연동 방식 및 기능

- **전용 플러그인 통합**: 저장소 내의 `.claude-plugin/` 폴더를 매개로 작동하는 designlang 플러그인은 Claude Code 콘솔 내부에서 사용할 수 있는 5가지 전용 슬래시 명령어를 등록한다.
  - `/extract`: 타겟 웹사이트 디자인 추출
  - `/grade`: 추출한 디자인 시스템 등급 판정 및 리포트 카드 생성
  - `/battle`: 두 사이트의 디자인 비교 분석
  - `/remix`: 추출한 디자인 시각적 언어 재생성
  - `/pack`: 디자인 패키징
- **상호 연동**: CLI 세션에서 [[contents/MCP_10494]]를 활용해 designlang이 제공하는 디자인 사양, 색상 쌍, 접근성 요소를 직접 검색하고 코드에 이식할 수 있으며, [[contents/Cursor_10494]]와 유사하게 에이전트 규칙 파일(`.emit-agent-rules`)을 참조하여 작업의 완성도를 높인다.
