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
- 4월 14일에 도서관에서 공부를 했던 것이 REST 자체를 더 깊게 알아 봐야겠다는 생각을 하게 된 발단이 됐다.
  - 마침 도서관에 있었으니 REST API에 대한 책이 없을까 찾아보았고, RESTful Web API 웹 API를 위한 모범 전략 가이드(레오나르드 리처드슨)라는 책을 빌렸다.
  - 처음 부분은 음 그렇군, 음 그런가? 하고 읽다가 더 점점 이상함을 느꼈다.
    - 도대체 지금 왜 이런 짓을 하고 있는 거지?? 라는 생각을 했다.
    - 예를 들자면, 그냥 JSON이 아니라 뭔가 좀 이상한 미디어 타입을 사용한다든지 그런 것들.
  - REST와 REST API는 꽤 다르다는 것을 정확하게 설명할 순 없었어도 대강 느낄 순 있었다.
  - 전부 다 읽진 못했다. 1장 ~ 4장 그리고 부록 C(필딩 논문 요약) 부분 정도만 읽었다.
  - 하지만 그만큼이라도 읽었기에 "그런 REST API로 괜찮은가?" 영상을 보고 조금이나마 이해를 할 수 있지 않았을까 생각이 든다.
- (아직 명확하게 정리하진 못했지만) API 자체에 대해서도 조금 더 생각해볼 기회가 됐다.
  - Spring의 @RestController 같은 것 때문에 REST API를 오해하고 있었던 지점도 있었다.
  - @RestController는 HTML 웹 페이지가 아니라 JSON을 반환하니까 JSON을 반환하면 REST API인가보다 하는 그런 순진한 생각 같은 것...

### (1)-(1) Spring의 @Controller와 @RestController
- @RestController는 간단히 말하면 @RequestMapping에 @ResponseBody를 붙여주는 역할을 한다.
  - 이로 인해 DispatcherServlet에서는 요청의 Accept 헤더를 확인 후 응답 객체를 MessageConverter를 통해 적절한 미디어 타입으로 변환 후 내보낸다.
    - 만약 @RestController가 아니라 @Controller라면 뷰 리졸버를 거쳐 프로젝트 내에서 해당하는 이름을 갖는 뷰 파일을 찾으려고 할 것이다.
- 위와 같이 @ResponseBody는 HTTP 명세를 잘 지키도록 도와주지만 @RestController가 그렇게 REST하진 않은 것 같다.
  - self-descriptive, HATEOAS를 지키도록 해주진 않기 때문이다.
  - @RestController의 Rest는 관용적인 의미라고 생각하고 넘어가면 될 것 같다.
- 하지만 어쨌든 간에 엔드포인트가 HTML 문서를 내보내지 않고, Accept에 맞는 문서(데이터)를 내보낸다는 점에서는 확실히 달라진다.
  - 이 덕분에 이 엔드포인트는 API의 엔드포인트가 된다.
  - 브라우저에서 사람을 위해 바로 보여줘야 하는 HTML 문서가 아닌
  - 기계가 처리해줘야하는 문서, 대표적인 예를 들자면 JSON 형식으로 문서를 준다.
- 즉 @RestController는 엔드포인트를 REST API의 엔드포인트로 만들어준다기보다는
  - HTTP 명세를 지킬 수 있는 API의 엔드포인트로 만들어준다고 표현하면 되지 않을까 싶다.

### (1)-(2) API가 HTML을 응답해도 상관 없다.
- <https://stackoverflow.com/questions/28480785/rest-api-why-no-html-instead-of-json>
- <https://softwareengineering.stackexchange.com/questions/207835/is-it-ok-to-return-html-from-a-json-api>
- 다만 응답을 받는 쪽에서 파싱하기 까다로워질 뿐

### (2) REST API가 유행어로 번진 이유에 대한 개인적인 생각
- 20일 발표 때 K-DEVCON 강성욱 멘토님께서 얘기하셨듯 REST API의 유행 배경에는 마이크로서비스 아키텍처의 대두가 있었던 것 같다.
  - <https://devopedia.org/application-programming-interface> 여기서도 이를 지적하고 있다.
- 하지만 MSA가 아닌 모놀리식 아키텍처를 사용하는 서비스에서는 REST API를 사용할 필요가 없는가?
  - 모놀리식 아키텍처에서 REST API를 사용할 필요가 없다면 왜 조그만 서비스를 제작할 때도 REST API를 말하는 사람들이 있는가?
  - 왜 MSA와는 상관도 없는 아키텍처를 가진 서비스에서 자꾸 자기들은 REST API를 사용한다고 주장하는 걸까?
- 누군가 얘기하는 것을 한 번도 본 적은 없지만 REST API가 유행어로 번진 다른 이유는 클라이언트 사이드 렌더링 기술의 유행 때문이 아닐까 생각한다.
  - 단지 추측일 뿐이긴 하다...
  - 클라이언트 사이드 렌더링의 유행에 따라 웹 어플리케이션 서버는 HTML 문서가 아닌 JSON 같은 문서를 주는 API 서버가 되어야했다.
  - 그리고 이 API 서버 개발에 뭔가 멋있는 이름을 주고 싶어서 REST라는 단어를 끌어온 것이 아닌가... 그런 생각이 든다.
    - HTML로 웹 페이지를 주는 게 아니라 데이터를 주게 되면서 API를 사용하게 됐고
    - API인데 RPC도 아니고, SOAP도 아니고, graphQL도 아니다.
    - 그럼 뭐지?
    - REST API라는 게 HTTP에 충실한 API라고 하네?
    - 우리도 HTTP 명세 잘 준수하잖아?
    - 그러면 우리 API도 REST API네!
    - 이런 흐름일까...?
-  정말로 개인적인 생각일 뿐이지만, 단순히 클라이언트 사이드 렌더링을 위해 API 서버의 API가 REST할 필요는 거의 없다고 생각한다.
  - 같은 회사 내의 프론트-백끼리 소통이 잘 되면 된다.
  - 빠른 개발을 할 때 REST 제약 조건을 신경 쓰는 것은 오히려 독일 수도 있을 것 같다.
- 만약 MSA를 사용하여 각 컴포넌트끼리 호출해야하는 상황이라면 REST 제약 조건의 필요성이 있지 않을까 생각은 들지만 잘 모르겠다.
  - 직접 MSA를 개발하고 사용해봐야 알 것 같다.

### 정리하자면
- 로이 필딩의 말을 가져와서 변형해서 말해보자면 REST 제약 조건이 가장 필요한 상황은
  - 불특정 다수의 통제할 수 없는 클라이언트들에게 API를 제공하는 경우가 아닐까 생각한다.
- 생각을 정리하자면
  - 클라이언트 사이드 렌더링 때문에 간단히 파싱할 수 있는 JSON 같은 문서를 주는 API 서버가 필요하다면 HTTP 명세에 충실한 API라면 충분하다.
  - 모놀리식 구조라면 굳이 REST 제약 조건에 집착할 필요성은 적은 것 같다. 물론 여력이 있다면 신경쓸 수도 있겠지만.
  - 모놀리식 구조더라도 불특정 다수의 통제할 수 없는 클라이언트들이 있다면 REST 제약 조건 준수를 생각해볼 필요가 있을 것 같다.
- **정말 마지막으로 정리하자면 서비스의 규모가 작고, 모놀리식 구조이고, 클라이언트가 프론트 애플리케이션 하나밖에 없다면 REST는 생각하지 않는 게 나은 것 같다.**

---

## 1. 참고 자료

### 주요 참고 자료
- [Fielding, Roy Thomas, Architectural Styles and the Design of Network-based Software Architectures. Doctoral dissertation, 2000](https://ics.uci.edu/~fielding/pubs/dissertation/top.htm)

- [RESTful Web API 웹 API를 위한 모범 전략 가이드, 레오나르드 리처드슨 외 3인, 2015](https://product.kyobobook.co.kr/detail/S000001033018)
  - 도서관에서 대여해서 읽었음

- [Day1, 2-2. 그런 REST API로 괜찮은가, 유튜브 NAVER D2 채널, 2017.11.20.](https://youtu.be/RP_f5dMoHFc?si=A8t7BbqP3XkQ8Mes)

### HTTP 관련 명세
- [RFC 1738 Uniform Resource Locators\(URL\)](https://datatracker.ietf.org/doc/html/rfc1738)
- [RFC 2616 Hypertext Transfer Protocol -- HTTP/1.1](https://datatracker.ietf.org/doc/html/rfc2616)
  - 개정 이전인 RFC 2068이 1997년에 나왔지만 보통 1999년의 RFC 2616을 HTTP/1.1로 부르는 듯하다.
  - 그리고 RFC 2616은 2014년에 RFC 7230 ~ 7235 문서에 의해 obsoleted 되었다.
    - RFC 7230은 Message Syntax and Routing, 7231은 Semantics and Content, 7232는 Conditional Requests, 7233은 Range Requests, 7234는 Caching, 7235는 Authentication에 대한 내용이다.
    - 위 중에서 내가 궁금할만한 정의들은 [RFC 7231 Semantics and Content](https://datatracker.ietf.org/doc/html/rfc7231)에서 찾아볼 수 있을 것 같다.
  - RFC 7230 ~ 7235 문서들은 2022년 RFC 9110 ~ 9112 문서들에 의해 Obsoleted 된 것 같다.
    - RFC 7231은 2022년 [RFC 9110 HTTP Semantics](https://datatracker.ietf.org/doc/html/rfc9110)에 의해 obsoleted 되었다.
  - [RFC 9113 HTTP/2](https://datatracker.ietf.org/doc/html/rfc9113)
    - HTTP/2는 9110 ~ 9112 직후 받아들여진 것 같다.
  - PATCH 메서드는 [RFC 5789 PATCH Method for HTTP](https://datatracker.ietf.org/doc/html/rfc5789)에 정의되어 있다.
- [RFC 2119 Key words for use in RFCs to Indicate Requirements Levels](https://datatracker.ietf.org/doc/html/rfc2119)
  - MUST, SHOULD, MAY 등의 일상 용어들을 RFC에서 무슨 의미로 사용하는지 정의해두었다.
- [RFC 5741 RFC Streams, Headers, and Boilerplates](https://datatracker.ietf.org/doc/html/rfc5741)
  - RFC 자체에 대한 내용인 듯하다.

### 그 밖의 참고 내용
#### 로이 필딩 2000년 박사 논문 관련
- 네이버 블로그 Do :: At one's fingertips_ 중 Roy Fielding 의 REST API 논문 번역 및 이해 시리즈 및 관련 글
  - [\[REST API\] Roy Fielding 의 REST API 논문 번역 및 이해\(1\)](https://m.blog.naver.com/aservmz/222234406469)
  - [\[REST API\] Roy Fielding 의 REST API 논문 번역 및 이해\(2\)](https://m.blog.naver.com/aservmz/222235368718)
  - [\[REST API\] Roy Fielding 의 REST API 논문 번역 및 이해\(3\)](https://m.blog.naver.com/aservmz/222240139709)
  - [\[REST API\] Roy Fielding 의 REST API 논문 번역 및 이해\(4\)](https://m.blog.naver.com/aservmz/222241342922)
  - [\[Spring\] 32. 스프링 RESTful 웹 서비스 이해 및 개발환경 구성하기](https://m.blog.naver.com/aservmz/222282710254)

#### 위키백과
  - [한국어 위키백과](https://ko.wikipedia.org/wiki/REST)
  - [영어 위키백과](https://en.wikipedia.org/wiki/REST)

#### 참고한 자료들 더 정리 중

---

## 2. 결론(...?)
### REST API 만들지 마세요
- HTTP API를 굳이 REST하게 만들 필요가 있을까?
  - 자료를 찾아보면서 절실히 느꼈던 점이었다.
  - 엔드포인트 이름을 지을 때 리소스를 고려한다든지, HTTP 명세를 잘 준수한다든지 이런 건 지키면 대개 좋을 것 같다는 느낌적인 느낌이 있다.
  - 하지만 REST API가 정말 REST 하려면 지켜야하는 self-descriptive나 HATEOAS가 정말로 많은 서비스에 필요할까?
    - 굳이 필요하지 않다는 게 내 개인적의 의견이다.

### don't waste youre time arguing about REST
- 로이 필딩은 2008년에 자신의 블로그의 [REST APIs must be hypertext-driven](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven)라는 글에 다음과 같이 적었다.
  - Please try to adhere to them or choose some other buzzword for your API.
  - 위에서 them는 하이퍼텍스트 제약조건을 말한다.
- 그리고 2015년 발표에서는 [발표 자료](https://roy.gbiv.com/talks/201511_Fielding_REST_CF.pdf)에 다음과 같은 말을 적어두었다.
  - REST emphasizes evolvability to sustain an uncontrollable system.
  - If you think you have control over the system or aren't interested in evolvability, don't waste your time arguing about REST.
- 그리고 2017년 발표에서는 [발표 자료](https://roy.gbiv.com/talks/201709_Fielding_REST.pdf)에 다음의 말을 적어두었다.
  - Because REST has become a BUZZWORD
  - There's nothing particularly wrong with that... unless you happen to be me or working with me
- 이제는 로이 필딩 마저 REST가 단지 유행어라는 점을 인정했으니, 모두가 행복해졌고 HTTP를 적당히 잘 쓰는 API라면 REST API라고 불러도 되는 걸까?
  - 그렇다면 REST 개념을 몰라도 될까?
  - 더욱이 내가 일하는 회사의 API에 REST 제약 조건이 필요하지 않다면 REST 같은 건 신경쓰지 않아도 되는 걸까?

### JSON 상하차 인력이 되지 않으려면
- 물론 신입 개발자에게 JSON 요청 받기 응답 하기 외의 다른 일을 할 선택권 자체가 없을 수 있겠다만...
  - 그래도 더 넓은 시야로 웹 개발을 바라보면 좋지 않을까 생각이 든다.
- 국비 캠프에 참여하면서 훈련생들이 뭔가 근본적인 것을 놓치고 있다는 생각이 들 때가 있었다.
  - 요청 받고 요청에 따라 DB에서 데이터 꺼내서 적절히 처리한 후 잘 내보내긴 하는데...
  - 그런데 그 다음은...?
  - 물론 캠프의 커리큘럼 문제일 수도 있겠다만...
- 웹 서비스가 자체가 근본적으로 분산 시스템이라는 것
  - 특히 분산 하이퍼미디어 시스템이라는 것
  - 웹의 역사를 살펴봤을 때 웹의 핵심은 하이퍼미디어(혹은 하이퍼텍스트)라는 것을 아는 것은 확실히 웹을 바라보는 데에 도움이 되지 않을까 한다.

---

## 3. REST (스타일을 참고한 HTTP 기반) API(라고 통상적으로 불리는 것의) 설계 가이드

- 통상적으로 REST API라고 불리(지만 사실은 REST 제약 조건을 만족하지 않)는 API를 설계할 때는 다음의 두 가지를 생각하면 될 것 같다.
  - 리소스 개념을 고려한 엔드포인트 이름 짓기
  - HTTP 메서드 관련 명세 준수
- 위 두 가지 조건에 대해서는 다음의 글들을 참고했다.
  - Microsoft REST API Guidelines [GitHub microsoft/api-guidelines 리포지토리](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)
  - [RESTful 웹 API 디자인](https://learn.microsoft.com/ko-kr/azure/architecture/best-practices/api-design) - Microsoft Learn Azure 아키텍처 센터
  - [REST API란 무엇인가](https://tech.trenbe.com/2022/01/16/rest-api.html) - 트렌비 기술 블로그
- 그리고 이는 Richardson Maturity Model의 Level 2에 해당하는 수준이라 보면 될 것 같다.

### 리소스 개념을 고려한 엔드포인트 이름 짓기
\*resource의 정의에 대해서는 "5. REST 관련 용어 및 기반 개념 살펴보기"에서 훑어볼 것이다.

### HTTP 메서드 관련 명세 준수

### 하지만 로이 필딩은...

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
### 결론 - 로이 필딩 2015년 발표 자료의 말 다시 보기

### 그러면 면접에서 REST API에 대해 물어보면 뭐라고 답해야 할까?

---
