# Remote Ops

원격 AI 작업 파이프라인 운영 문서.

## 구조

```
remote-ops/
├── prompts/           ← Cursor용 프롬프트 템플릿
│   ├── planning.md        기획/구조 설계
│   ├── design-fix.md      디자인/UI 수정
│   └── dev-task.md        개발/버그 수정
├── runbooks/          ← 운영 가이드
│   ├── remote-workflow.md       원격 작업 루틴
│   ├── deployment-checklist.md  배포 체크리스트
│   └── home-pc-setup.md        집 PC 세팅 가이드
└── logs/              ← 작업 로그
    └── 2026-03-10.md       파이프라인 구축 로그
```

## 파이프라인

```
폰 → ChatGPT 구조화 → Notion Inbox 저장
         ↓
AnyDesk 원격 접속 → Cursor 실행
         ↓
git push → Vercel 자동 배포 → 폰에서 확인
```

## 연결된 프로젝트

| 프로젝트 | GitHub | Vercel |
|---------|--------|--------|
| webscout-next | [dalgoms/webscout-next](https://github.com/dalgoms/webscout-next) | webscout-next-8veo.vercel.app |
