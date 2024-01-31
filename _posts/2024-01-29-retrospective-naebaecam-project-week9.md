---
title: 9주차 백오피스 프로젝트 KPT 팀 회고
author: rugii913
date: 2024-01-29 18:15:00 +0900
categories: [회고, 2024년 1월]
tags: [retrospective]
render_with_liquid: false
---

<img src="/assets/img/post-img/kpt-retrospective-naebaecamp-back-office-project.png" width="auto" alt="kpt-retrospective"/>
### Keep
- 프로젝트 초기 event storming 잘 진행 됨
- GitHub Project 이용한 프로젝트 관리
  - 다음에 다른 프로젝트에서도 사용해보면 좋을 듯합니다.
- project 구성할 때 함께 진행해서 프로젝트를 이해하기 수월했다.
- GitHub Pull Request 코드 리뷰
  - 다른 팀원들의 진행사항을 간단하게나마 파악할 수 있어서 좋았음
- API & ERD 작성
  - 각자 한번씩 API와 ERD 작성을 해봄으로써 만들려고자 하는 프로젝트를 이해하는 데에 도움이 되었다.
  - 각자 작성한 것을 통합함으로써 자신이 생각을 하지 못했던 부분에 대해 좀 더 알게 된 것 같다.

### Problem
- 와이어프레임, 화면 등 없음
  - 와이어프레임이나 기타 화면 등 없어서 의사소통할 때 애매했던 부분이 있었다.
- 일관성 있는 PR, commit message 컨벤션 필요
  - 다른 조 발표에서 보면, 팀만의 PR 내용, commit message 규칙을 정해서 PR, 커밋 시 이에 맞춰서 작성했습니다.
  - 다음 프로젝트에서는 PR 내용, commit message 규칙을 정해보고 싶네요.
- 테스트 코드 작성하지 못함
  - 시도는 했으나 결국 작성하지 못했음
- 베이스가 부족해서 개발 속도가 느렸다.
  - 나름대로 코드 개발을 진행했지만 부족한 부분이 있어 개발 속도가 느렸다.
- 코드 통합이 제대로 되지 않았다.
  - 각자 domain을 하나씩 맡아서 개발하다보니 코드가 일관성 있게 통합 안 된 부분이 있다.

### Try
- PR 시스템 활용
  - 다른 팀원이 작성한 코드를 보면서 팀 내에서나 개인적으로라도 이해하고 공부해보고 넘어가면 실력 향상에 도움이 될 듯
- 화면의 중요성
  - 첫날에 화면 스케치나 와이어프레임 작성하기
- 전체적인 프로젝트 도메인 훑기
  - event storming 함께 작성, API 명세, ERD 모두 각각 작성하는 방법이 아니더라도 개개인별로 프로젝트 전체 도메인에 대해 생각해보는 시간이 있으면 좋을 듯합니다.
  - 앞서 말한 event storming이나 API 명세 작성 등의 방식처럼 약간의 강제성이 부여될 필요도 있지 않을까 합니다.
- 이번 프로젝트 코드 보고 따라치기
  - 이번 프로젝트를 보고, 코드를 따라치면서 알아가는 시도가 필요한 것 같습니다.