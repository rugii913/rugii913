---
title: 최종 프로젝트 업무 정리
author: rugii913
date: 2024-04-08 12:02:00 +0900
categories: [회고, 2024년 4월]
tags: [retrospective]
render_with_liquid: false
---

## 이력서 준비 중 생각해볼 것

### 메타인지
- 구체적인 목표와 방향성: 내가 도달해야 하는 수준을 명확하게
- 나의 수준을 객관적으로 파악: **내가 채워야 하는 부분을 인지**하는 것
  - 부딪쳐봐야만 본인의 수준, 회사의 요구사항을 조금 더 구체적으로 알 수 있다.
  - 현직 개발자들에게 이력서 코칭 등도 받아보고...
  - 그래야 방향성과 그 수준 같은 목표를 설정할 수 있다.

### 이력서란?
- **내 경험**을 바탕으로 **무엇을 할 수 있는지** 풀어내는 글
  - (x) 했던 일 나열
  - (o) 했던 일을 재료로 **문제 해결 역량** 표현

### 이력서 준비
- (1) 우선 경험들 전부 모아서 단순 나열
  - GitHub 커밋, PR, 이슈, ...
  - 팀 회의록, 슬랙 메시지, ...
  - 블로그 글, 5분 기록 보드, ...
  - 테스트코드 커버리지
- (2) 경험 분류
  - 메인: 트러블 슈팅
    - 더 세부적으로 분류 가능 → 기능 vs. 개선 구분 / 화면별 구분
  - 서브: 간단한 기능 구현
- (3) 경험 다듬기: 골든 서클(Golden Circle) 이론에 따라
  - 목적(why): 왜, 목적, 필요성 / 문제 정의 시 이것을 문제라고 생각한 이유
  - 과정(how): 목적 달성(문제 해결)을 위해 시도한 구체적인 방법 + 그 과정에서의 고민과 생각
  - 결과(what): 방법의 결과물과 성과 output, outcome → 글 또는 숫자의 형태로
  - **핵심**: 정의한 문제에 대해 타인을 공감시키고, 제시한 해결 방안이 타당하다고 타인을 설득하기

## 경험 작성
- 소스는 간트 차트, PR, 이슈, 커밋, 5분 기록 보드, 회의록 등

### 수행 주체별 업무 나열
<details>
<summary>
팀 업무 전체
</summary>
<div markdown="1">
#### 팀 업무 전체
- 서비스 초기 기획 관련(공통)
  - 주제 선정
  - 정책 협의
  - 와이어프레임 작성
  - API 명세 작성
  - ERD 작성
- 서비스 중간 기획 관련
  - 추가 기능 협의(공통)
  - WBS 구성, 요구사항 명세서, 간트 차트(곽)
  - 리포지토리 리드미 작성(공통)
- 기본 CRUD API
  - 초기 엔티티 작성(공통)
  - 사용자 및 프로필
    - 뜨거운, 새로운 뒹글 페이지 목록 조회 API(최)
    - 프로필 이미지 업로드 API(노)
      - 프로필 기본 이미지 처리(노)
      - 프로필 이미지 등록 시 확장자 및 용량 제한(노)
    - 프로필 이력 관리 기능: Spring Data Envers(노)
    - 프로필 단건 조회 API(노)
    - 닉네임으로 뒹글 페이지 검색 API(최)
    - 랜덤 뒹글러 추천 API(최)
  - 사용자 인증
    - 소셜 로그인(+회원 가입) API(최)
    - Spring Security 초기 설정(최)
  - 뒹글(인증, 인가, 테스트 포함)
    - 뒹글 등록 API(김)
    - 뒹글 피드 화면 슬라이스 조회 API(곽)
    - 뒹글 피드 화면 팔로우하는 사람의 뒹글 슬라이스 조회 API(최)
    - 뒹글 개인 페이지 화면 슬라이스(뒹글+캐치) 조회 API(김)
  - 캐치(인증, 인가, 테스트 포함)
    - 캐치 등록 API(김)
    - 캐치 삭제 API(김)
  - 팔로우(인증, 인가, 테스트 포함)
    - 팔로우 등록 API(김)
    - 팔로우 목록 조회 API(김)
    - 팔로워 수 조회 API(김)
    - 팔로우 취소(삭제) API(김)
  - 공지사항(인증, 인가, 테스트 포함)
    - 공지사항 등록 API(김)
    - 공지사항 조회 API(김)
    - 공지사항 수정 API(김)
    - 공지사항 삭제 API(김)
  - 신고(인증, 인가, 테스트 포함)
    - 신고 엔티티 작성(곽)
    - 신고 등록 API(곽)
    - 신고 등록 시 신고 등록된 뒹글, 캐치 블락(노)
    - 신고 목록 조회 API(곽)
  - 알림(인증, 인가, 테스트 포함)
    - 알림 엔티티 작성(최)
    - 알림 목록 조회 API(김)
- 알림 SSE
  - 새 뒹글에 대한 알림(최)
  - 새 캐치에 대한 알림(최)
  - 피드 새 개시물 알림 SSE(최)
- 리팩토링: 기본 CRUD API 처리 개선 혹은 기존 기능 처리 개선
  - 뒹글러 목록 조회 관련 성능(부하) 테스트(최)
  - 예외 처리 공통화 리팩토링(최)
  - 소셜 로그인 기능에 따라 User 엔티티를 SocialUser 엔티티로 변경(최)
  - DooingleQueryDslRepositoryImpl 공통 부분 추출 리팩토링(최)
  - 뒹글 피드 API 응답 DTO 프로퍼티 수정(곽)
  - 알림 기능 반환 타입 리팩토링(최)
  - Spring Security filter chain 리팩토링(곽)
    - CORS 설정 리팩토링
    - 프로파일에 따라 다른 filter chain 구성, 스프링 액추에이터 요청 포트 은닉
  - SocialUser 엔티티에 userLink 프로퍼티 추가, userId를 body에 갖고 있는 DTO 수정(곽)
  - 프로필 기본 이미지 여부 확인 후 기본 이미지가 아닌 경우에만 이미지 url 가져오도록 리팩토링(최)
  - 뒹글러 목록 사이즈 환경 변수 제거 리팩토링(최)
  - NotificationResponse 리팩토링(최)
    - NotificationResponse DTO에서 userId 대신 userLink 프로퍼티사용
    - NotificationResponse의 cursor를 resourceId + 1로 수정
    - SSE 테스트코드에서 NotificationResponse 관련 에러 수정
  - SseEmitter 개수 제한: List 대신 Queue 사용(최)
  - 기타 API 리팩토링(곽)
    - userLink로 프로필 받아오는 API 추가
    - FollowDetailResponse DTO에서 non-nullable인 프로퍼티를 nullable로 수정: 이미지 url, 자기소개
    - 공지사항 응답 데이터에 누락된 notice의 id 추가
    - 팔로워 수 반환 메서드 오류 수정: PathVariable
    - CatchResponse의 deletedAt이 null이 아닌 경우 content를 null 처리
    - Notification 엔티티에서 ownerUserLink 프로퍼티(컬럼) 제거
    - NotificationResponse와 NotificationSseResponse 분리, NotificationSseResponse에서는 userLink 데이터 보내지 않음
  - 뜨거운 뒹글러 기능 캐시 적용(최)
  - 동시성 제어를 위한 분산락 적용(노)
  - 중복 신고 방지, 신고 누적 시 자동으로 블락 기능(최)
  - 분산락 적용 시 트랜잭션과 얽히는 부분 리팩토링(곽)
- 인프라(곽)
  - 환경 변수 노출 방지 및 스프링 부트 설정 관리
    - 테스트 환경 embedded redis 설정
  - 백엔드 CI 워크플로우
  - 프론트엔드 CD 워크플로우
  - 백엔드 CD 워크플로우
  - 백엔드 배포 스크립트
  - 인프라 구성
    - IAM 및 security group 설정
    - Route53 도메인 연결(백, 프론트), SSL 인증서 처리
    - EC2, ELB, S3, RDS 등 시작 및 연결
- 모니터링(곽)
  - 프로메테우스, 그라파나: Docker Compose로 구성하여 EC2 인스턴스에서 실행
- 화면 및 프론트엔드(곽)
  - 화면 프로토타입
  - 화면 퍼블리싱
  - 프론트엔드 코드 작성
</div>
</details>

<details>
<summary>
내 업무
</summary>
<div markdown="1">
#### 내 업무
\* 위 팀 업무 전체에서 내 업무만 추린 내용
- 서비스 초기 기획 관련(공통): 주제 선정, 정책 협의, 와이어프레임 작성, API 명세 작성, ERD 작성
- 서비스 중간 기획 관련(공통): 추가 기능 협의, 리포지토리 리드미 작성
  - WBS 구성, 요구사항 명세서, 간트 차트(곽)
- 기본 CRUD API
  - 초기 엔티티 작성(공통)
  - 뒹글(인증, 인가, 테스트 포함)
    - 뒹글 피드 화면 슬라이스 조회 API(곽)
  - 신고(인증, 인가, 테스트 포함)
    - 신고 엔티티 작성(곽)
    - 신고 등록 API(곽)
    - 신고 목록 조회 API(곽)
- 리팩토링: 기본 CRUD API 처리 개선 혹은 기존 기능 처리 개선
  - 뒹글 피드 API 응답 DTO 프로퍼티 수정(곽)
  - Spring Security filter chain 리팩토링(곽)
    - CORS 설정 리팩토링
    - 프로파일에 따라 다른 filter chain 구성, 스프링 액추에이터 요청 포트 은닉
  - SocialUser 엔티티에 userLink 프로퍼티 추가, userId를 body에 갖고 있는 DTO 수정(곽)
  - 기타 API 리팩토링(곽)
    - userLink로 프로필 받아오는 API 추가
    - FollowDetailResponse DTO에서 non-nullable인 프로퍼티를 nullable로 수정: 이미지 url, 자기소개
    - 공지사항 응답 데이터에 누락된 notice의 id 추가
    - 팔로워 수 반환 메서드 오류 수정: PathVariable
    - CatchResponse의 deletedAt이 null이 아닌 경우 content를 null 처리
    - Notification 엔티티에서 ownerUserLink 프로퍼티(컬럼) 제거
    - NotificationResponse와 NotificationSseResponse 분리, NotificationSseResponse에서는 userLink 데이터 보내지 않음
  - 분산락 적용 시 트랜잭션과 얽히는 부분 리팩토링(곽)
- 인프라(곽)
  - 환경 변수 노출 방지 및 스프링 부트 설정 관리
    - 테스트 환경 embedded redis 설정
  - 백엔드 CI 워크플로우
  - 프론트엔드 CD 워크플로우
  - 백엔드 CD 워크플로우
  - 백엔드 배포 스크립트
  - 인프라 구성
    - IAM 및 security group 설정
    - Route53 도메인 연결(백, 프론트), SSL 인증서 처리
    - EC2, ELB, S3, RDS 등 시작 및 연결
- 모니터링(곽)
  - 프로메테우스, 그라파나: Docker Compose로 구성하여 EC2 인스턴스에서 실행
- 화면 및 프론트엔드(곽)
  - 화면 프로토타입
  - 화면 퍼블리싱
  - 프론트엔드 코드 작성
</div>
</details>

### 내 업무 분류 - 서브 vs. 메인

#### 서브 업무
- 서비스 초기 기획 관련(공통): 주제 선정, 정책 협의, 와이어프레임 작성, API 명세 작성, ERD 작성
- 서비스 중간 기획 관련(공통): 추가 기능 협의, 리포지토리 리드미 작성
- 기본 CRUD API
  - 초기 엔티티 작성(공통)
  - 뒹글: 뒹글 피드 화면 슬라이스 조회 API
  - 신고: 신고 엔티티 작성, 신고 등록 API, 신고 목록 조회 API
- 리팩토링: 기본 CRUD API 처리 개선 혹은 기존 기능 처리 개선
  - 뒹글 피드 API 응답 DTO 프로퍼티 수정
  - Spring Security filter chain 리팩토링
    - CORS 설정 리팩토링
    - 프로파일에 따라 다른 filter chain 구성, 스프링 액추에이터 요청 포트 은닉
  - SocialUser 엔티티에 userLink 프로퍼티 추가, userId를 body에 갖고 있는 DTO 수정
  - 기타 API 리팩토링
    - userLink로 프로필 받아오는 API 추가
    - FollowDetailResponse DTO에서 non-nullable인 프로퍼티를 nullable로 수정: 이미지 url, 자기소개
    - 공지사항 응답 데이터에 누락된 notice의 id 추가
    - 팔로워 수 반환 메서드 오류 수정: PathVariable
    - CatchResponse의 deletedAt이 null이 아닌 경우 content를 null 처리
    - Notification 엔티티에서 ownerUserLink 프로퍼티(컬럼) 제거
    - NotificationResponse와 NotificationSseResponse 분리, NotificationSseResponse에서는 userLink 데이터 보내지 않음
- 인프라
  - 환경 변수 노출 방지 및 스프링 부트 설정 관리
  - 백엔드 CI 워크플로우
  - 프론트엔드 CD 워크플로우
  - 백엔드 CD 워크플로우
  - 백엔드 배포 스크립트
  - 인프라 구성
    - IAM 및 security group 설정
    - Route53 도메인 연결(백, 프론트), SSL 인증서 처리
    - EC2, ELB, S3, RDS 등 시작 및 연결
- 화면 및 프론트엔드
  - 화면 프로토타입
  - 화면 퍼블리싱

#### 메인 업무
- 서비스 중간 기획 관련: WBS 구성, 요구사항 명세서, 간트 차트
- 리팩토링
  - 알림 SSE 오류 분석 및 해결: 프론트엔드 업무와도 연관
  - 분산락 적용 시 트랜잭션과 얽히는 부분 리팩토링
- 인프라
  - 테스트 환경 embedded redis 설정(서브 업무 중 환경 변수 노출 방지 및 스프링 부트 설정 관리 관련)
  - 백엔드 배포 스크립트를 실행 환경에 따른 분리하는 작업 중 트러블 해결
  - 인프라 구성 중 웹 개발 전반에 대해서 생각해본 지점들 풀어내기(위 서브 업무 중 인프라 구성 관련 고민한 지점들 종합)
- 모니터링: 프로메테우스, 그라파나: Docker Compose로 구성하여 EC2 인스턴스에서 실행
  - 모니터링 이후 인프라 변경 결정
- 화면 및 프론트엔드
  - 프론트엔드 코드 작성 중 느낀 API 명세의 중요성, 백엔드-프론트엔드의 협업 필요성: 요청해서 변경한 코드도 있고, 시간 관계 상 직접 변경한 코드도 있음
  - 프론트에 노출되는 프로퍼티 변경 필요성 userId → userLink

