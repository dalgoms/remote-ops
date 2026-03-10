# 원격 작업 워크플로우

## 루틴 순서

### 1. 아이디어 단계 (모바일)
1. 폰에서 아이디어 작성 (ChatGPT / 메모앱)
2. ChatGPT로 구조화 → Cursor용 프롬프트 생성
3. Notion Work Inbox에 저장
   - Title: 작업 제목
   - Project: 해당 프로젝트 선택
   - Request Type: planning / design-fix / dev-task / bug-fix / refactor / deploy
   - Priority: urgent / high / medium / low
   - Prompt: ChatGPT가 생성한 구조화된 프롬프트

### 2. 실행 단계 (원격 PC)
1. AnyDesk로 PC 접속
2. Notion Work Inbox에서 작업 확인
3. 프롬프트 템플릿 활용 (`ops/prompts/` 참고)
   - `planning.md` → 기획/구조 설계
   - `design-fix.md` → 디자인/UI 수정
   - `dev-task.md` → 개발/버그 수정
4. Cursor에 프롬프트 붙여넣기 → 실행
5. Cursor가 코드 수정 완료

### 3. 배포 단계
1. 변경 사항 확인 (git diff)
2. 커밋 & 푸시 (git add → commit → push)
3. Vercel 자동 배포 대기
4. 배포 결과 확인

### 4. 기록 단계
1. Notion Work Inbox 상태 업데이트 (Status → Done)
2. Output Link에 배포 URL 기록
3. Notes에 핵심 변경사항 메모

## 프로젝트별 경로

| 프로젝트 | 로컬 경로 | GitHub 레포 | Vercel URL |
|---------|----------|------------|-----------|
| webscout-next | `cursor test/webscout-next` | [dalgoms/webscout-next](https://github.com/dalgoms/webscout-next) | webscout-next-8veo.vercel.app |
| timbel-homepage | `cursor test/timbel-homepage` | - | - |
| fvg-monitor | `cursor test/fvg-stock-monitor` | - | - |
| content-factory | `cursor test/content-factory` | - | - |

> 위 표의 GitHub/Vercel URL은 실제 값으로 채워주세요.

## 실전 활용 시나리오

### 시나리오 1: 출퇴근 중 아이디어

1. 폰에서 ChatGPT에게 아이디어를 말한다
2. "ops/prompts/planning.md 형식으로 정리해줘"라고 요청
3. 결과물을 Notion Work Inbox에 복붙 저장 (Project, Priority 설정)
4. 나중에 PC 앞에서 Cursor에 프롬프트 붙여넣기 → 실행
5. `git push` → Vercel 자동 배포 → 폰에서 결과 확인

### 시나리오 2: 긴급 버그 수정

1. 폰에서 AnyDesk로 PC 접속
2. Cursor 열기 → 버그 내용 입력
3. Cursor가 수정 완료
4. 터미널에서 `git add . && git commit -m "fix: 버그 설명" && git push`
5. Vercel 자동 배포 → 폰에서 결과 확인

### 시나리오 3: 작업 일괄 처리

1. 하루 종일 Notion Inbox에 아이디어/수정 요청을 쌓아둔다
2. 저녁에 PC 앞에 앉아서 Inbox를 Priority 순으로 정렬
3. 하나씩 Cursor에 프롬프트 붙여넣기 → 실행
4. 모든 작업 완료 후 한꺼번에 push → 배포
5. Inbox 상태를 Done으로 업데이트, Output Link 기록

## Notion Inbox 활용 팁

- **프롬프트는 길게 감정적으로 쓰지 말고** 템플릿 구조(Task/Context/Goal/Constraints/Todo)로 정리
- **Project 태그를 반드시 선택** — 나중에 필터링할 때 필수
- **Status 관리 습관화** — Not Started → In Progress → Done
- **Output Link에 배포 URL 기록** — 나중에 결과 추적 가능

## 멀티 PC 운영 가이드

두 대 이상의 PC(회사/집)에서 작업할 때 지켜야 할 규칙:

### 작업 시작 전

```bash
git pull origin master
```

다른 PC에서 push한 변경사항을 먼저 받아야 충돌을 방지할 수 있다.

### 작업 끝날 때

```bash
git add .
git commit -m "작업 요약"
git push origin master
```

push하지 않으면 다른 PC에서 이어서 작업할 수 없다.

### 어느 PC에서 작업하든 동일한 흐름

```
git pull → 작업 → git push → Vercel 자동 배포
```

> 집 PC 세팅 방법은 `ops/runbooks/home-pc-setup.md` 참고

## 체크 포인트

- [ ] PC 절전 모드 해제되어 있는가?
- [ ] AnyDesk 정상 동작 중인가?
- [ ] Git 인증 상태 유효한가?
- [ ] Vercel 연동 정상인가?
- [ ] 작업 시작 전 `git pull` 했는가?
