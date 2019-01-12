---
layout: "post"
title: "git reset --hard origin/master 한거 취소하기"
date: "2019-01-10 23:58"
tag : git
comments : true
---

# 오늘 한 실수
- local에 있는 파일을 github에 push하려고 하는데 reject가 되었다. remote가 잘 못 되었나 해서 확인해보았지만 사실 맥주먹고 알딸딸한 상태라 계속 실수하다
- 구글 검색해서 찾은걸 그냥 카피페이스트 해서 실행시켜 버린 순간
- 그 밑에 있는 설명을 읽어보 "만약, 로컬에 있는 모든 변경 내용과 확정본을 포기하려면, 아래 명령으로 원격 저장소의 최신 이력을 가져오고, 로컬 master 가지가 저 이력을 가리키도록 할 수 있어요"
- 그리고 이미 내 로컬은 텅비어있던 github를 역push 해버려서 readme 파일만 남은 상태..
- 실수한 걸 깨닫고 돌리려고 다시 구글링
- 그래서 찾은 & 그리고 몸으로 배운 오늘의 git 명령어

### git reflog

### git reset --hard HEAD@{#} (#은 위의 reflog에서 찾은 것)
