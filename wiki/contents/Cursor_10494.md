# Cursor (Document 10494)

Cursor는 [[contents/designlang_10494]]의 디자인 시스템 추출 데이터를 적극적으로 연동하여 활용할 수 있는 대표적인 AI 코드 에디터이다.

## 연동 방식 및 기능

- **에이전트 규칙 파일 제공**: designlang 실행 시 `--emit-agent-rules` 옵션을 적용하면, Cursor와 [[contents/Claude Code_10494]] 등에서 바로 참조할 수 있는 시스템 프롬프트/규칙 파일을 함께 출력한다.
- **[[contents/MCP_10494]] 리소스 연동**: designlang이 실행하는 MCP 서버를 통해 Cursor 내부의 AI 에이전트가 타겟 웹사이트에서 추출한 디자인 토큰, 명암비 색상 쌍, 레이아웃 컨텍스트 정보를 직접 조회하고 코드 생성에 반영하도록 연결할 수 있다.
