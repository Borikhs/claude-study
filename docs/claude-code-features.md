# Claude Code 주요 기능 정리

> 이슈 #1 관련 학습 정리 문서. 정기 발표 자료로 활용 예정.

---

## 1. Slash Commands

Claude Code 내에서 `/` 로 시작하는 명령어로 빠르게 기능을 실행할 수 있다.

| 명령어 | 설명 |
|---|---|
| `/help` | 사용 가능한 명령어 목록 확인 |
| `/clear` | 현재 대화 컨텍스트 초기화 |
| `/commit` | 변경사항을 git commit |
| `/review-pr` | PR 코드 리뷰 |
| `/fast` | Fast 모드 토글 (빠른 응답) |

사용자 정의 slash command(skill)도 `.claude/commands/` 에 추가해 사용할 수 있다.

---

## 2. Hooks

특정 이벤트 발생 시 자동으로 실행되는 쉘 명령어를 설정할 수 있다.

- 설정 위치: `.claude/settings.json` 또는 `settings.local.json`
- 활용 예시:
  - 파일 저장 시 자동 포맷
  - 도구 실행 전/후 로깅
  - 커밋 전 lint 자동 실행

```json
{
  "hooks": {
    "user-prompt-submit": ["echo 'prompt submitted'"]
  }
}
```

---

## 3. MCP (Model Context Protocol) 서버 연동

외부 도구나 데이터 소스를 Claude에 연결하는 프로토콜.

- Claude가 직접 외부 API, DB, 파일 시스템 등에 접근 가능
- 활용 예시:
  - Context7: 최신 라이브러리 문서 조회
  - GitHub MCP: 이슈/PR 직접 관리
  - Slack MCP: 메시지 전송 자동화

---

## 4. 멀티 에이전트 워크플로우

하나의 작업을 여러 에이전트가 병렬 또는 순차적으로 처리하는 방식.

- `Agent` 도구로 서브에이전트를 실행
- 독립적인 작업은 병렬로, 의존성이 있는 작업은 순차적으로 실행
- 활용 예시:
  - 코드 작성 에이전트 + 코드 리뷰 에이전트
  - 탐색 에이전트 + 구현 에이전트

```
메인 에이전트
├── 서브에이전트 A (탐색)
├── 서브에이전트 B (구현)  ← 병렬 실행 가능
└── 서브에이전트 C (리뷰)  ← A, B 완료 후 실행
```
