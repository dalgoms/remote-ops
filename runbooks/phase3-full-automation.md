# Phase 3: 풀오토 에이전트

Notion 등록 -> 코딩 -> 배포 -> 결과 회신까지 전부 자동.

---

## 전제 조건

- Phase 1 완료 (Telegram + Make.com)
- Phase 2 완료 (Claude Code Action + GitHub Issue 연동)

---

## Step 1: 자동 머지 규칙 활성화

### GitHub 레포 설정

1. https://github.com/dalgoms/webscout-next/settings → **General**
2. **Pull Requests** 섹션에서:
   - "Allow auto-merge" 체크
   - "Automatically delete head branches" 체크

### 사용법

- Claude가 만든 PR에 `auto-merge` 라벨 추가 → 빌드 성공 시 자동 머지
- Priority가 urgent/high인 작업은 라벨 안 붙이고 수동 리뷰

---

## Step 2: 배포 결과 회신

이미 `deploy-notify.yml` 워크플로우가 설치되어 있다.
Secrets에 `TELEGRAM_BOT_TOKEN`과 `TELEGRAM_CHAT_ID`가 등록되어 있으면 자동 동작.

### 알림 내용
- 레포 이름
- 커밋 메시지
- 작성자
- 배포 URL

---

## Step 3: Notion 자동 업데이트

### Make.com 시나리오 추가

새 시나리오를 만든다:

1. Trigger: **GitHub** → **Watch Pull Requests** (merged)
2. Action 1: **Notion** → **Update a Database Item**
   - Work Inbox에서 PR 제목과 매칭되는 항목 찾기
   - Output Link: Vercel 배포 URL
   - Notes: PR URL + 변경 요약
3. Action 2: **Telegram** → **Send a Message**
   - 완료 알림 + 배포 URL

---

## Step 4: 에러 핸들링

### Make.com 시나리오 추가

1. Trigger: **GitHub** → **Watch Check Runs** (failed)
2. Action: **Telegram** → **Send a Message**
   - 에러 내용 + Issue 링크

### Claude 실패 시

Claude가 해결하지 못한 작업은:
1. PR에 `needs-human` 라벨 자동 추가
2. Telegram으로 "수동 작업 필요" 알림

---

## 최종 흐름

```
폰에서 Notion Inbox 등록
    ↓ (Make.com, 즉시)
Telegram 알림 + GitHub Issue 생성
    ↓ (Claude Code Action, 1-5분)
Claude가 코드 작성 → PR 생성
    ↓ (자동 머지, 빌드 성공 시)
Vercel 자동 배포
    ↓ (deploy-notify, 즉시)
Telegram 배포 완료 알림 (URL 포함)
    ↓ (Make.com)
Notion Inbox 자동 업데이트 (Done + Output Link)
```

전체 소요 시간: 작업 등록 후 5-15분 이내 배포 완료.

---

## 안전장치 요약

| 상황 | 동작 |
|------|------|
| 빌드 성공 + auto-merge 라벨 | 자동 머지 → 배포 |
| 빌드 성공 + 라벨 없음 | PR 대기 (수동 리뷰) |
| 빌드 실패 | Telegram 에러 알림 |
| Claude 실패 | needs-human 라벨 + 알림 |
| Priority urgent/high | 자동 머지 안 함, 리뷰 요청 |
