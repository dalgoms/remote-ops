# 배포 체크리스트

## 배포 전

- [ ] 로컬에서 빌드 성공 확인 (`npm run build`)
- [ ] 린트 에러 없음 확인
- [ ] 주요 기능 동작 확인
- [ ] 환경변수 (.env) 확인 — 새로 추가된 것 있는지
- [ ] 변경 파일 목록 리뷰

## 커밋 & 푸시

```bash
git add .
git status          # 변경 파일 확인
git commit -m "작업 요약"
git push origin main
```

## 배포 확인

- [ ] Vercel 대시보드에서 빌드 상태 확인
- [ ] 배포 URL 접속하여 결과 확인
- [ ] 모바일에서도 접속 테스트
- [ ] 콘솔 에러 없는지 확인

## 배포 후

- [ ] Notion Work Inbox 상태 업데이트 → Done
- [ ] Output Link에 배포 URL 기록
- [ ] 문제 발견 시 Notes에 기록 → 다음 작업으로 등록

## 롤백이 필요한 경우

```bash
git log --oneline -5          # 최근 커밋 확인
git revert HEAD               # 마지막 커밋 되돌리기
git push origin main          # 되돌린 상태 배포
```

## 주의사항

- main 브랜치에 직접 push하는 구조이므로 커밋 전 반드시 빌드 확인
- .env 파일은 절대 커밋하지 않기
- 큰 변경은 브랜치를 나눠서 PR로 진행
