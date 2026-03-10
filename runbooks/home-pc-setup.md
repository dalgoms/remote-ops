# 집 PC 세팅 가이드

회사 PC와 동일한 원격 작업 파이프라인을 집 PC에도 구축하는 가이드.
이 문서를 위에서 아래로 순서대로 따라하면 된다.

---

## 1. AnyDesk 설치

1. https://anydesk.com/en/downloads 에서 Windows 버전 다운로드
2. 설치 후 표시되는 **내 주소(9자리 숫자)** 기록
3. 설정(톱니바퀴) → **보안** → **무인 접속 비밀번호** 설정
4. 아이폰 AnyDesk 앱에서 새 주소로 접속 테스트

> 완료 후 이 문서 하단의 "내 기기 정보"에 주소를 기록해두자.

---

## 2. 절전 모드 해제

원격 접속은 PC가 켜져 있어야 동작한다.

1. 설정 → 시스템 → 전원 및 절전
2. **화면 끄기**: 사용 안 함 또는 30분 이상
3. **절전 모드**: **사용 안 함**으로 설정

---

## 3. Git 설치

1. https://git-scm.com/download/win 에서 다운로드 → 설치 (기본 옵션 OK)
2. 설치 확인:

```bash
git --version
```

3. 사용자 정보 설정:

```bash
git config --global user.name "Seyoung"
git config --global user.email "seyoung8967@gmail.com"
```

---

## 4. GitHub CLI 설치 및 인증

1. PowerShell(관리자)에서 실행:

```bash
winget install --id GitHub.cli
```

2. 설치 후 새 터미널 열고 인증:

```bash
gh auth login --web --git-protocol https
```

3. 브라우저에서 표시되는 코드 입력 → GitHub에서 **Authorize** 클릭
4. 인증 확인:

```bash
gh auth status
```

`Logged in as dalgoms`가 표시되면 성공.

---

## 5. Node.js 설치

1. https://nodejs.org 에서 LTS 버전 다운로드 → 설치
2. 설치 확인:

```bash
node --version
npm --version
```

---

## 6. 프로젝트 Clone

작업 폴더를 만들고 프로젝트를 받는다:

```bash
mkdir "C:\Users\Owner\Downloads\cursor test"
cd "C:\Users\Owner\Downloads\cursor test"
git clone https://github.com/dalgoms/webscout-next.git
cd webscout-next
npm install
```

---

## 7. 환경변수 설정

프로젝트 루트에 `.env.local` 파일을 만들고 아래 내용을 넣는다:

```
COLLECT_API_URL=https://webscout-agent.vercel.app/api/collect
```

> .env.local은 Git에 포함되지 않으므로 PC마다 직접 만들어야 한다.

---

## 8. 빌드 테스트

```bash
npm run build
```

에러 없이 완료되면 세팅 끝.

---

## 9. Cursor 설치

1. https://cursor.sh 에서 다운로드 → 설치
2. webscout-next 폴더 열기
3. `.cursor/rules/` 폴더의 룰 파일이 자동 적용됨

---

## 10. 완료 후 확인 체크리스트

- [ ] AnyDesk 설치 및 무인 접속 비밀번호 설정
- [ ] 절전 모드 해제
- [ ] Git 설치 및 사용자 정보 설정
- [ ] GitHub CLI 인증 (`gh auth status` → Logged in as dalgoms)
- [ ] Node.js 설치
- [ ] 프로젝트 clone 및 `npm install`
- [ ] `.env.local` 생성
- [ ] `npm run build` 성공
- [ ] Cursor 설치
- [ ] 아이폰에서 AnyDesk 접속 테스트

---

## 내 기기 정보

| 기기 | AnyDesk 주소 | 비고 |
|------|-------------|------|
| 회사 PC | 1 71 130 239 | 메인 작업 머신 |
| 집 PC | (설치 후 기록) | |
| 아이폰 | 115 266 931 | 모바일 접속용 |

---

## 멀티 PC 운영 시 주의사항

두 대 이상의 PC에서 같은 프로젝트를 작업할 때 반드시 지켜야 할 규칙:

### 작업 시작 전 — 항상 pull

```bash
git pull origin master
```

다른 PC에서 push한 변경사항을 먼저 받아야 충돌을 방지할 수 있다.

### 작업 끝날 때 — 항상 push

```bash
git add .
git commit -m "작업 요약"
git push origin master
```

push하지 않으면 다른 PC에서 이어서 작업할 수 없다.

### 충돌이 발생하면

```bash
git pull origin master
# 충돌 파일 수동 해결
git add .
git commit -m "Resolve merge conflict"
git push origin master
```
