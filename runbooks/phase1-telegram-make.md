# Phase 1: Telegram + Make.com 알림 자동화

Notion Inbox에 작업을 등록하면 Telegram으로 알림이 오는 구조.

---

## Step 1: Telegram Bot 만들기 (5분)

1. Telegram에서 **@BotFather** 검색 → 대화 시작
2. `/newbot` 입력
3. 봇 이름 입력 (예: `Work Inbox Alert`)
4. 봇 username 입력 (예: `seyoung_inbox_bot`)
5. **API 토큰**이 발급됨 → 복사해두기 (예: `7123456789:AAF...`)
6. 발급된 봇 링크를 눌러서 봇에게 `/start` 보내기

### Chat ID 확인

봇에게 아무 메시지나 보낸 후, 브라우저에서:

```
https://api.telegram.org/bot{토큰}/getUpdates
```

응답에서 `"chat":{"id": 숫자}` 부분의 숫자가 본인의 Chat ID.

> 토큰과 Chat ID를 메모해두자.

---

## Step 2: Make.com 시나리오 생성 (15분)

### 2-1. Make.com 접속

1. https://www.make.com 로그인
2. **Create a new scenario** 클릭

### 2-2. Trigger 설정: Notion

1. **Notion** 모듈 추가
2. **Watch Database Items** 선택
3. Connection: Notion 계정 연결 (처음이면 Integration 생성 필요)
4. Database: **Work Inbox** 선택
5. Watch: **Created Items** 선택

### 2-3. Notion Integration 연결 (처음 한 번만)

1. https://www.notion.so/my-integrations 접속
2. **+ New integration** 클릭
3. 이름: `Make Automation`
4. Capabilities: **Read content** 체크
5. Submit → **Internal Integration Secret** 복사
6. Notion에서 Work Inbox 페이지 열기 → 우측 상단 `...` → **Connections** → `Make Automation` 추가

### 2-4. Action 설정: Telegram

1. Notion 모듈 옆에 **+** 클릭
2. **Telegram Bot** 모듈 추가
3. **Send a Message** 선택
4. Connection: Bot Token 입력
5. Chat ID: Step 1에서 확인한 Chat ID 입력
6. Message Text:

```
📋 새 작업 등록됨

제목: {{Title}}
프로젝트: {{Project}}
유형: {{Request Type}}
우선순위: {{Priority}}
프롬프트: {{Prompt}}
```

### 2-5. 실행

1. 시나리오 하단의 **Scheduling** → ON
2. **Run once** 로 테스트
3. Notion Inbox에 새 항목 추가 → Telegram 알림 확인

---

## 완료 확인

- [ ] Telegram Bot 생성됨
- [ ] Bot Token + Chat ID 확보
- [ ] Make.com 시나리오 생성됨
- [ ] Notion Inbox 새 항목 → Telegram 알림 수신 확인
