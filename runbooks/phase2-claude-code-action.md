# Phase 2: GPT Codex Action 자동 코딩

Notion에 작업 등록 -> GitHub Issue 자동 생성 -> GPT Codex가 코드 작성 -> PR 생성.

---

## Step 1: OpenAI API 키 발급 (5분)

1. https://platform.openai.com 접속 → 로그인
2. **API Keys** → **Create new secret key**
3. 키 복사해두기 (sk-...)

---

## Step 2: GitHub 레포에 Secret 등록 (5분)

1. https://github.com/dalgoms/webscout-next/settings/secrets/actions 접속
2. **New repository secret** 클릭
3. 아래 3개 등록:

| Name | Value |
|------|-------|
| `OPENAI_API_KEY` | sk-... (OpenAI API 키) |
| `TELEGRAM_BOT_TOKEN` | Phase 1에서 발급받은 봇 토큰 |
| `TELEGRAM_CHAT_ID` | Phase 1에서 확인한 Chat ID |

---

## Step 3: 워크플로우 확인

이미 레포에 3개의 워크플로우가 설치되어 있다:

### codex.yml — AI 자동 코딩
- Issue에 `ai-task` 라벨 붙이면 GPT Codex가 자동 실행
- 댓글에 `@codex` 멘션하면 Codex가 응답
- 코드 작성 후 자동 PR 생성

### deploy-notify.yml — 배포 알림
- master에 push 시 Telegram으로 배포 완료 알림
- 배포 URL + 커밋 메시지 포함

### auto-merge.yml — 자동 머지 (Phase 3)
- `auto-merge` 라벨이 붙은 PR은 빌드 성공 시 자동 머지

---

## Step 4: Make.com 시나리오 확장 (15분)

Phase 1의 시나리오에 GitHub Issue 생성을 추가한다.

### 기존 시나리오 수정

1. Make.com에서 Phase 1 시나리오 열기
2. Telegram 모듈 뒤에 **+** 클릭
3. **GitHub** 모듈 추가 → **Create an Issue** 선택
4. Connection: GitHub 계정 연결
5. Repository: `dalgoms/webscout-next`
6. Title: `{{Title}}`
7. Body:

```
## Notion Inbox Task

**Project:** {{Project}}
**Type:** {{Request Type}}
**Priority:** {{Priority}}

---

## Prompt

{{Prompt}}

---

@codex 위 프롬프트를 실행해주세요.
```

8. Labels: `ai-task` 추가

### 결과

Notion Inbox 등록 → Telegram 알림 + GitHub Issue 생성 → GPT Codex 자동 코딩 → PR 생성

---

## Step 5: 테스트

1. Notion Work Inbox에 새 작업 등록
2. Telegram 알림 수신 확인
3. GitHub Issues 탭에서 Issue 생성 확인
4. GPT Codex가 PR 생성하는지 확인 (API 키 등록 후)
5. PR 리뷰 → Merge → Vercel 자동 배포

---

## 사용법

### 자동 실행 (Notion 경유)
Notion Inbox에 등록만 하면 끝. Make.com이 Issue 생성 → GPT Codex가 PR 생성.

### 수동 실행 (GitHub 직접)
1. GitHub Issues에서 직접 Issue 생성
2. `ai-task` 라벨 추가 또는 본문에 `@codex` 포함
3. GPT Codex가 자동 실행

### GPT Codex에게 추가 지시
Issue 댓글에 `@codex 수정해줘: ...` 형태로 추가 요청 가능.

---

## 비용

- OpenAI API: 소규모 작업 기준 월 $5~10
- GitHub Actions: 퍼블릭 레포 무료
- Make.com: 무료 플랜 1,000 ops/월
