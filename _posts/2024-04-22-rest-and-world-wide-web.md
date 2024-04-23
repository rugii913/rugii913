---
title: REST와 World Wide Web
author: rugii913
date: 2024-04-22 13:42:00 +0900
categories: [개발+외]
tags: [network, REST]
render_with_liquid: false
---

- **\"제목은 REST와 World Wide Web으로 하겠습니다. 그런데 이제 REST API를 곁들인...\"**라는 제목으로 발표한 내용과 스터디에서 나왔던 이야기들을 정리한 글입니다.

- 사실이 아닌 주관적인 생각, 의견, 인상 비평도 일부 있을 수 있습니다.
  - 틀린 내용 반박 환영합니다. rugii913@gmail.com

## 목차
<details>
<summary>
목차 열어보기
</summary>
<div markdown="1">
- 1. 참고 자료
- 2. 결론(...?)
- 3. REST (스타일을 참고한 HTTP 기반) API(라고 통상적으로 불리는 것의) 설계 가이드
- 4. API 비교: REST API - SOAP - RPC
- 5. REST 관련 용어 및 기반 개념 살펴보기
- 6. REST의 시작: Architectural Styles and the Design of Network-based Software Architectures
- 7. 정말로 REST API를 만들고 싶다면...: 웹 vs. 웹 API: 의미적 차이라는 제약 조건
- 8. Richardson Maturity Model: REST 제약 조건 관점의 웹 서비스 성숙도 모델
- 9. 결론
</div>
</details>

---

## 들어가기에 앞서
- 처음에 이 주제로 발표를 해야겠다고 생각했을 때는
  - \"3. REST (스타일을 참고한 HTTP 기반) API(라고 통상적으로 불리는 것의) 설계 가이드\" 이 부분을 중점적으로 공부해야하지 않을까 생각했었다.
  - 로이 필딩의 논문을 읽어보려고 시도하고, 요약본을 대강 찾아보긴 했지만 대충 훑어보고 그런가보다 하고 넘겼다.
  - 아마도 인터넷 검색한 자료만 이용했다면 "그런 REST API로 괜찮은가?" 같은 영상을 봤더라도 대충만 훑기만 하고 넘겨버렸을 것 같다.
- 4월 14일에 도서관에서 공부를 했던 것이 발단이었다.
  - 마침 도서관에 있었으니 REST API에 대한 책이 없을까 찾아보았고, RESTful Web API 웹 API를 위한 모범 전략 가이드(레오나르드 리처드슨)라는 책을 빌렸다.
  - 처음 부분은 음 그렇군, 음 그런가? 하고 읽다가 더 점점 이상함을 느꼈다.
    - 도대체 지금 왜 이런 짓을 하고 있는 거지?? 라는 생각을 했다.
    - 예를 들자면, 그냥 JSON이 아니라 뭔가 좀 이상한 미디어 타입을 사용한다든지 그런 것들.
  - REST와 REST API는 꽤 다르다는 것을 정확하게 설명할 순 없었어도 대강 느낄 순 있었다.
  - 전부 다 읽진 못했다. 1장 ~ 4장 그리고 부록 C(필딩 논문 요약) 부분 정도만 읽었다.
  - 하지만 그만큼이라도 읽었기에 "그런 REST API로 괜찮은가?" 영상을 보고 조금이나마 이해를 할 수 있지 않았을까 생각이 든다.

---

## 1. 참고 자료
- Fielding, Roy Thomas, Architectural Styles and the Design of Network-based Software Architectures. Doctoral dissertation, 2000
  - <https://ics.uci.edu/~fielding/pubs/dissertation/top.htm>
- 네이버 블로그 Do :: At one's fingertips_ 중 Roy Fielding 의 REST API 논문 번역 및 이해 시리즈 및 관련 글
  - (\[REST API\] Roy Fielding 의 REST API 논문 번역 및 이해\(1\))[https://m.blog.naver.com/aservmz/222234406469]
  - (\[REST API\] Roy Fielding 의 REST API 논문 번역 및 이해\(2\))[https://m.blog.naver.com/aservmz/222235368718]
  - (\[REST API\] Roy Fielding 의 REST API 논문 번역 및 이해\(3\))[https://m.blog.naver.com/aservmz/222240139709]
  - (\[REST API\] Roy Fielding 의 REST API 논문 번역 및 이해\(4\))[https://m.blog.naver.com/aservmz/222241342922]
  - (\[Spring\] 32. 스프링 RESTful 웹 서비스 이해 및 개발환경 구성하기)[https://m.blog.naver.com/aservmz/222282710254]
- 위키백과
  - https://ko.wikipedia.org/wiki/REST
  - https://en.wikipedia.org/wiki/REST

---

## 2. 결론(...?)

---

## 3. REST (스타일을 참고한 HTTP 기반) API(라고 통상적으로 불리는 것의) 설계 가이드

---

## 4. API 비교 - REST(REpresentational state transfer) - RPC(Remote Procedure Call) - SOAP(Simple Object Access Protocol)

- 인터페이스
  - interface, surface, face
    - 계약, 상호작용, 캡슐화
    - 캡슐화: 구현을 숨긴다.
      - (1) 숨긴다 - 오용하지 않도록
      - (2) 구현을 - 노출된 부분이 변경되지 않는다면, 클라이언트는 수정 필요 x, 구체적인 것의 변경 전파 x
  - API
  - (주관적)왜 프로토콜이 아니라 인터페이스?

### 공통점
- 계약: 특정한 프로토콜 집합을 사용
- 상호작용: 서로 다른 구성 요소가 요청과 응답으로 통신
- 캡슐화: 통신 상대의 내부 작동 방식에 대해서는 신경쓰지 않음

---

## 5. REST 관련 용어 및 기반 개념 살펴보기
- 웹 = 월드 와이드 웹
  - 웹 서비스
  - 네트워크: 그 인프라 / 키워드: 종단 시스템, ...
- resource, state, representation? 간략한 요약은 나중에
  - state
    - 전형적인 클라이언트-서버 구조 생각해보기
    - 응용프로그램의 state는 기본적으로 메모리에 저장
    - 이 state에 영속성 부여 필요 시, 파일 시스템 사용 혹은 영속성 전문적 처리 인프라 사용
    - 영속성 외 여러 장점 때문에, 별도의 외주 업체 DBMS를 끌어들이는 것
    - 애플리케이션 상태: 어떤 페이지에 와 있는가?
  - stateless(무상태)
    - 상태를 저장하지 않는 것이 아님
      - 클라이언트와 서버 모두 상태를 저장하는데, 각기 다른 종류의 상태를 저장함
      - 클라이언트가 어떤 상태인지 서버가 전혀 신경쓰지 않는다는 뜻
    - HTTP 세션은 매우 짧음
      - 서버는 클라이언트 애플리케이션 상태(클라이언트가 현재 있는 페이지)를 전혀 알지 못함
      - 클라이언트는 리소스 상태에 대해 직접적인 컨트롤를 전혀 할 수 없고 모든 것은 다 서버 쪽에서 이루어짐
      - 애플리케이션 상태는 클라이언트 측에서 관리하지만, 서버는 가능한 상태 전이를 나타내는 표현(ex. HTML 문서)을 보내 조작 가능
      - 리소스 상태는 서버에서 관리하지만 클라이언트 역시 서버에 원하는 새 상태를 설명하는 표현(ex. HTML 폼)을 보내 조작 가능
  - HATEOAS(Hypermedia As The Engine Of Application State): 애플리케이션 상태의 엔진으로서 하이퍼미디어
    - '연결'(링크), '하이퍼미디어 제약 조건'이라고 간단하게 말할 수도 있을 것
      - 하이퍼미디어: HTML 링크, 폼 같은 것들을 부르는 일반 용어로 생각
    - 서버가 다음에 무엇을 할 수 있는지 클라이언트에 설명하는 기법
      - 직관적으로 얘기하자면, "웹을 링크를 따라가고 폼을 작성하는 것으로 사용"하는 것을 뜻함

### 애플리케이션 상태 vs. 서버 상태

---

## 6. REST의 시작: Architectural Styles and the Design of Network-based Software Architectures

### REST라는 용어: REpresentational State Transfer

### REST의 배경
- cf. http 1.1은 1997년 1월 발표(RFC 2068) <https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP> (MDN HTTP의 진화)
  - http 1.1 개정: 1999년 6월 RFC 2616
  - http 1.1 또 개정: 2014년 6월 RFC 7230 - RFC 7235
  - cf. ajax는 1999년 3월 발표, 
- 처음에 소개한 논문에서 웹이 경쟁 솔루션보다 **왜** 더 잘 동작하는지를 설명하며 REST라는 용어를 탄생시킴
  - 핵심은 링크 같은 요소
    - 어떤 URL 입력해야하는지 별도 문서로 설명이 필요하다면 연결의 원칙이나 자기 서술 메시지 원칙을 어기는 것임

#### REST의 시작이 된 논문
- 구성
  - 1장 소프트웨어 아키텍처
  - 2장 네트워크 기반 응용프로그램 아키텍처
  - 3장 네트워크 기반 아키텍처 스타일
  - 4장 웹 아키텍처 설계: 문제와 통찰
  - 5장 표현 상태 전송(REST)
  - 6장 경험과 평가

### 그렇다면 REST API의 배경은???

---

## 7. 정말로 REST API를 만들고 싶다면...: 웹 vs. 웹 API: 의미적 차이라는 제약 조건

---

## 8. Richardson Maturity Model: REST 제약 조건 관점의 웹 서비스 성숙도 모델

---

## 9. 결론

---
