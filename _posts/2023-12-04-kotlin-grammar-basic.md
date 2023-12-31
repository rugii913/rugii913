---
title: Kotlin 기초 강의 정리
author: rugii913
date: 2023-12-04 01:24:00 +0900
categories: [Kotlin, 문법]
tags: [Kotlin, grammar]
render_with_liquid: false
---

**나름대로 열심히 적긴 했는데, 맘에 안 드는 부분이 많아서 나중에 전체적으로 다시 정리하게 될 것 같다.**
  - 참고할 자료는 아토믹 코틀린 책 및 공식 문서, 구현하면서 인터넷에서 본 자료 등

**가까운 시일 내에 다시 공부할 부분은**
  - 이넘, null 관련, 람다 관련 전부, 코루틴, 영역함수(인라인?)

#### Kotlin 문법 간단하게 참고할만한 한글 자료 <https://gold.gitbook.io/kotlin/>

## 1주차: Kotlin을 시작하기 전에 알아야할 내용
### 프로그래밍이란
- 프로그래밍이란?
- 컴퓨터 명령어란?
- 프로그래밍 언어란? 사람 - 컴퓨터 간 소통을 위한 목적
  - 왜 프로그래밍 언어가 많을까? - 특정한 목적에 적절하게 사용하기 위해

### Kotlin 소개
- 구글 안드로이드 앱 개발 관련
- Java와의 관련성, Java와 비교
  - 자료형 추론 기능
  - 더 간결한 코드
  - NPE를 컴파일 시점에 잡는다.
- Kotlin 특징 중 중요한 부분
  - JVM 언어와 호환
  - 직관적이고 간결한 문법
  - null 처리에 높은 안정성

### 안드로이드와 Kotlin
- 왜 Kotlin을 사용하는가? - 구글에서 Kotlin을 권장하는 이유 문서 참고 // Java와의 비교
  - <span style="color: red">간결하고 안전하게 비동기 처리 수행 - 심화 파트에서 살짝 훑어 봄</span>

### 개발 환경 설정
- IntelliJ IDEA(JVM 언어 관련 개발 환경) / Android Studio(Android 앱 개발 환경)


> \(troubleshooting 실패\)   
cf. Android studio 설치 중 HAXM fail log  
=\> https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows#user-content-Installing_Intel_HAXM_on_Windows_via_Android_Studio  
위 매뉴얼 따라서 재설치 시도, 하지만 다시 실패  
=\> 로그 확인 C:\Users\rugii\AppData\Local\Temp\haxm_install-20231204_0159.log, haxm_install-20231204_0209.log, haxm_install-20231204_0219.log, haxm_install-20231204_0224.log로 기록되어 있었음  
=\> 빙 검색 Copilot 답변 및 https://github.com/intel/haxm/wiki/Windows-System-Configurations 확인  
=\> 제어판 \> 프로그램 \> Windows 기능 켜기/끄기 > Hyper-V 클릭 후 확인 > 다시 시작
명령어 sc.exe start intelhaxm
여전히 에러 떠서 Windows 하이퍼바이저 플랫폼 체크 후 다시 시작
참고 https://bluemoonworld.tistory.com/entry/Intel%C2%AE-HAXM-installation-failed-%EC%97%90%EB%9F%AC-%EB%B0%9C%EC%83%9D-%EB%B0%8F-%ED%95%B4%EA%B2%B0-with-Android-Studio
<!-- quote 블록은 강제 개행 나오기 전까지 안 끝남  -->

> \(troubleshooting 실패\)   
다른 글 다시 참고  
https://velog.io/@wordi/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%8A%A4%ED%8A%9C%EB%94%94%EC%98%A4-HAXM-%EC%84%A4%EC%B9%98-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0  
=\> 전부 끄고 다시 시도  
그래도 여전히 명령어 sc.exe start intelhaxm 하면 안 됨...  
위 블로그 글에서 하듯 Android Studio 들어가서  
Settings \> Language & Framworks \> Android SDK \> SDK Tools 눌러보니  
Intel x86...\(HAXM installer\) - <span style="color: red">Deprecated</span>라 돼있어서 무시하기로 함...

- 왼쪽 Project 패널에서 Project, Packages, ... 등이 아니라 Android 선택해서 패키지 보면 간단

### 유용한 단축키
- IDE 관련
  - 한 줄 지우기 =\> 나는 Shift + Delete 사용함
  - 한 줄 주석 처리 및 해제 =\> Ctrl + /
  - 자동 포커싱 =\> Esc // 다른 패널, 창에서 에디터 창으로 돌아오는 것을 말함
  - 전체 찾기(Find in Files) =\> Ctrl + Shift + F // Shift 두 번 누르는 것과는 다름 - 파일 내에서 문자열 찾기
- 개발 관련
  - 저장 =\> Ctrl + S
  - 라인 드래그 =\> Shift + 방향키
  - 문장 최상단/최하단 이동 =\> Home/End

---

## 2주차: Kotlin 프로그래밍의 기초
### 코딩 컨벤션
- 다른 사람들도 이해하기 쉽게 코드를 작성하는 규칙
  - **왜 필요한가? 코드 가독성, 유지보수, 협업**
  - 프로그래밍 언어마다 다름
- Kotlin에서 자주 사용하는 표기
  - 변수 이름 =\> camelCase
  - 상수 이름 =\> snake_case
  - 클래스 이름 =\> Pascal_Case

### 출력과 입력
- 프로그램의 출력이란? 프로그램에서 다른 장치로 데이터를 전송하는 행위
- 프로그램의 입력이란? 다른 장치로부터 데이터를 불러와서 프로그램에서 확인하는 행위
- 실습
  - 모니터에 정보를 출력해보기
  - 키보드에서 입력한 텍스트를 불러오기
  - 키보드에서 입력한 정수를 불러오기

### 자료형
- 자료형: 자료를 저장할 수 있는 적절한 형태
  - 프로그램의 모든 정보를 데이터 또는 자료라고 한다.
  - 각 기본적인 자료형마다 정해진 bit 단위 크기가 있다. + 담을 수 있는 데이터가 정해져있다.
- 자료형은 적절히 사용해야 함
  - 컴퓨터 하드웨어 사양
  - 자료의 정확한 표현

### 변수와 상수
- var 키워드 - 변수
  - var 변수이름:자료형 = 값
  - var 변수이름 = 값 (자료형 생략 가능)
- val 키워드 - 상수
- print(~)에서 변수나 상수를 ${~}로 불러와서 출력할 수 있음

### 연산자의 종류
- 산술 연산자: +. -, *, /, %
- 대입 연산자: = // 오른쪽에 있는 값을 왼쪽의 변수 혹은 상수에 대입
- 복합대입 연산자: +=, -=, *=, /=, %= // 산술 연산자와 대입 연산자를 복합
- 증감 연산자: ++, -- // cf. 변수 왼쪽에 붙으면 전위 연산, 변수 오른쪽에 붙으면 후위 연산
- 비교 연산자: >, >=, <, <=, ==, !=

### 조건식의 사용
- if, else
  - 비교 연산자에 의한 조건식을 explicitly 사용
- when, else
  - ==이 숨어있다고 생각할 수 있다.
- if는 최악의 경우에 모든 조건을 비교하는 단점이 있음  
\+ else if가 많아질 때는 when을 사용하는 게 가독성도 좋음

### 반복문의 사용
- for
  - for(요소 in 리스트)
  - for(인덱스 in 시작값 until 마지막 값 + 1) // exclusive end
==\> for(인덱스 in 시작값..마지막 값) // inclusive end
*cf. 여기서 인덱스는 var가 아니라 val이다. 함부로 할당, 재대입할 수 없음
- while
  - while(조건식)
- break 반복문에서 탈출하고 다음 블록으로 넘어감
- continue 이후의 코드를 실행하지 않고 다음 반복으로 넘어감

---

## 3주차: 객체 지향 프로그래밍의 기초
### 메서드 설계
- 메서드? 소스코드에 이름을 붙이는 것
  - 특정한 로직을 가지는 소스코드에 별명(이름)을 붙임
  - 선언할 때: fun \[메서드 이름\]([변수명]:[자료형], ...):반환자료형 { 소스코드 로직 }
    - 반환 자료형이 없을 때는 생략하거나 : Unit으로 명시
  - 왜? 언제? 어디에 사용하는가
    - 로직을 추상화해놓고 상황에 맞게 실행
    - 코드의 재사용성 높임
  - 사용할 때: \[메서드 이름\]([인자], ...)
    - 반환 자료형이 Unit(이거나 생략되었다면) return 값이 없음
    - 반환 자료형이 따로 정해져있으면 메서드 호출한 곳에서 return 값을 돌려받음
cf. fun main(~) {~}도 함수이다. 코틀린은 프로그램의 시작점인 main 함수를 알아서 찾아서 실행한다.

### 클래스 설계
- Object Oriented Programming - 프로그래밍 패러다임 중 하나
  - 5개 키워드: 클래스, 추상화, 캡슐화, 상속, 다형성
  - Kotlin에서도 모든 것이 클래스 형태이므로 객체화 가능
    - 필요한 데이터를 추상화시켜서 상태와 행위를 가진 객체를 만든다.
    - 객체들 간 적절한 결합을 통해 유지보수를 쉽게 한다.
- 클래스 - class 키워드
  - 프로그램 각 요소의 설계도
  - 하나의 파일에 여러 클래스가 존재할 수도 있음
  - 클래스에는 정보(프로퍼티)와 행위(메서드)를 작성함  
class \[클래스명\] {  
&nbsp;&nbsp;정보1  
&nbsp;&nbsp;정보2  
&nbsp;&nbsp;행위1  
&nbsp;&nbsp;행위2  
}
- 특별한 클래스들이 필요한 경우가 있다.
  - data class - 정보(프로퍼티)만 갖고 있는 클래스가 필요한 경우
    - 정보, 생성자만 만들면 몇 가지 메서드들을 자동으로 만들어준다.
  - sealed class - 상속을 제한적으로만 사용할 때
  - object class - Java의 static 대신 사용하는 키워드: 프로그램을 실행하는 동시에 인스턴스화

### 생성자의 활용
- 생성자: 클래스를 실체화할 때  최초로 실행할 로직
- 기본 생성자 vs. 명시적 생성자
- 명시적 생성자 =\> 주 생성자 vs. 부 생성자
  - 주 생성자: 클래스 옆에 있음, init {~} 사용
    - 한 가지 형태로 클래스를 실체화할 때 사용
  - 부 생성자: 클래스 안에 있음, constructor(~){~} 사용
    - 여러 형태로 클래스를 실체화할 때 사용

### 객체의 활용
- 객체: 모든 인스턴스를 포함함, 클래스 타입으로 선언된 것
  - 클래스 타입으로 선언된 객체라는 것을 실체화해서 메모리에 올린 것을 **인스턴스**라고 한다.
  - 인스턴스를 통해서 클래스라는 설계도에 있는 정보와 행위에 접근할 수 있다.
  - 인스턴스는 메모리 공간을 차지한다.
- 클래스를 실체화한다는 것은?
  - 정보와 행위를 작성한 클래스를 실체화해서 프로그램에 로딩 (메모리에 적재)
  - 정확하게 정보와 행위를 메모리에 적재하는 게 아니라 그 위치정보를 메모리에 로딩
  - 프로그램은 **객체의 위치정보를 변수**에 저장하고, 필요할 때 **참조**한다.
    - "어떤 객체의 주소에 가서"를 **\".\"**으로 이해하면 된다.

### 상속
- 상속: 클래스 간의 관계를 더 끈끈하게
  - 공통 요소들이 있다면 부모/자식으로 구분해서 상속관계를 만들 수 있다.
  - Kotlin의 class는 기본적으로 final이 생략되어있다고 생각하면 된다.
    - open 키워드를 붙이면 상속 가능하게 된다.
    - sealed 키워드를 붙이면 상속은 가능하지만  
그 subclass는 동일한 파일에 정의되어야 하며, sealed class는 private만 가능하다.  
\*cf. sealed class를 사용할 때의 장점 참고: <https://codechacha.com/ko/kotlin-sealed-classes>
- 왜 사용하는가?
  - 다형성을 구현
  - 공통된 부분을 변경해야하는 경우 부모 클래스만 변경하는 것으로 끝낼 수 있다.
- 방법:
  - 부모 클래스는 open class로 만들고
  - 자식 클래스 선언 시 class [자식클래스명]: [부모클래스 생성자 호출] { 로직 }  
=> 부모 클래스에 명시 생성자만 있다면, 자식클래스 선언 시 부모클래스 생성자 호출 부분에 필요한 인자를 넘겨주는 부분이 있어야 한다.

### 오버라이딩
- 오버라이딩: 상속받은 부모 클래스의 정보(프로퍼티)나 행위(메서드)를 재설계
  - 주로 **행위(메서드)를 재설계**
- 왜 사용하는가?
  - 공통적인 내용은 부모 클래스에서 관리하면서, subclass의 개성을 살릴 수 있다.
  - **어차피 오버라이딩으로 재설계한다면 왜 애초에 상속을 했어야 하는가?**  
=\> 상속으로 클래스들 간의 관계를 만들고, 일관성을 유지  
=\> 유지보수 용이, 재사용성 제고
- 방법
  - 부모 클래스에 있는, 상속받게 하고 싶은 메서드의 선언 fun 앞에 open을 붙임  
open fun \[메서드명\]\(~\) { 부모 로직 }
  - 자식클래스에서 override fun \[메서드명\]\(~\) { 재설계 로직 }

### 오버로딩
- 오버로딩: 메서드 이름이 같지만, 매개변수의 개수와 자료형이 다른 메서드를 여러 개 만들 수 있다.
  - 반환 자료형은 오버로딩에 영향 x  
=\> 메서드 이름 같고, 매개변수 개수, 자료형 같은데 반환형 다른 메서드로 오버로딩할 순 없다.
- 왜 필요한가?
  - 오버로딩이 없다면,  
ex. Int를 더하는 메서드, Double를 더하는 메서드를 같은 이름으로 사용할 수 없다.  
&nbsp;&nbsp;&nbsp;&nbsp;매개변수의 타입으로 충분히 구별할 수 있는데도...

### 인터페이스
- 인터페이스: 공통적으로 필요한 기능을 외부에서 추가해줄 수 있다.
  - Kotlin 역시 다중 상속 불가, 공통점을 모두 상속으로 처리할 수 없다.
  - 추상메서드: 메서드의 로직이 존재하지 않고, 이름만 선언된 것  
=\> fun \[메서드명\]\(\)  
    - 인터페이스는 이 추상 메서드를 이용함
    - 원칙적으로는 인터페이스는 추상 메서드만 작성
- 왜 필요한가?
  - 상속을 통해 공통으로 묶기에는 부적절하지만
  - 어쨌든 공통으로 묶을 수 부분이 있는 경우 인터페이스를 구현하게 하여 공통 기능을 추가해준다.
- 방법:
  - interface 선언 \> (추상) 메서드 선언
  - 구현할 class에서는 class \[클래스명\]: \[인터페이스명\] { 로직 - **인터페이스의 추상 메서드를 반드시 override** }
  - 만약 상속과 구현을 동시에 한다면 class \[자식클래스명\]: \[부모클래스 생성자 호출\], \[인터페이스명\] { 로직 }  

---

## 4주차: 객체지향 프로그래밍의 심화
### 접근제한자
- 접근제한자: 변수나 메서드로의 접근을 제한할 수 있다.
  - public, private, internal, protected 키워드들
  - 접근: 객체를 이용해서 변수나 메서드를 호출할 수 있는지 여부  
=\> 앞 객체의 활용 강의에서 \".\"을 사용한 것을 생각하면 됨
- 범위 살펴보기
  - 프로젝트: 최상단 개념 - 모듈, 패키지, 클래스를 포함
  - 모듈: 프로젝트 아래 개념 - 패키지, 클래스를 포함
  - 패키지: 모듈 아래의 개념 - 클래스를 포함
- 각 접근제한자 키워드별 접근 범위
  - public: 명시하지 않으면 public - 어디서나 접근 가능
  - private: 같은 클래스만 접근 가능
  - internal: 같은 모듈에서만 접근 가능
  - protected: 같은 클래스 및 subclass만 접근 가능
- 왜 필요한가?
  - 접근 권한을 통해 데이터에 무분별한 접근을 막음  
=\> 향후 유지보수하기 용이  
\*cf. 처음 만드는 간단한 수준의 앱은 public, private 아는 것만으로 충분할 것

### 예외 처리의 활용 및 필요성
- 프로그램 실행 도중 발생하는 예외(런타임 에러)를 적절하게 처리
  - 실행 도중 프로그램이 비정상적으로 종료되는 것을 막음
  - 미리 예외를 생각하고 소스코드를 작성해야 안정성이 높은 프로그램이다.
- Kotlin의 예외처리 방법
  - (1)try-catch-finally
  - (2)throw
- 예외 처리 필요성  
ex. 숫자를 입력받아서 더하는 프로그램인데 문자를 입력받았다면 예외 처리  
ex. 사진을 다운로드 받는 도중 인터넷이 끊긴다면 예외 처리
ex.(finally 사용) USB, GPS 등 자원을 사용하는 코드는 사용 후에 연결을 끊어줌

### 지연초기화
- 왜 필요한가?
  - 변수나 상수의 값을 나중에 초기화할 수 있다.
  - Kotlin은 원래 안정성을 위해 설계할 때 반드시 변수 값 초기화하도록 권장
  - (1) 하지만 클래스 설계시 초기 값 정의하기 애매한 경우
  - (2) 혹은 저사양 환경에서 효율적으로 메모리 사용 - 필요할 때 값을 넣는다.
- 변수는 lateinit 키워드로 지연 초기화
  - ex. lateinit var name: String
  - 초기화됐는지 확인하려면
    - this::[정보명].isInitialized
    - ::[정보명].isInitialized
    - 이렇게 확인하지 않고, try catch로 예외처리하면 너무 길어진다.
- 상수는 lazy 키워드로 지연 초기화
  - ex. val name: String by lazy
  - 사용할 때에서야 val이 초기화되어 메모리 자원을 아낌 

### 널 세이프티
- Kotlin은 자료형에 null 여부를 명시
  - NPE로부터 안전한 설계를 위해 지원하는 키워드들  
=\> ?, !!, ?., ?:  
=\> 이 중에서 !!는 사용 자제할 것 - 강제로 null이 아님을 주장함
  - \*cf. null을 저장하지 않고 설계하기 위해 lateinit var를 사용할 수도 있음
- \[Type?\]: null을 저장할 수 있는 Type임을 명시
- \[객체!!\]: 해당 Type? 타입인 객체가 null이 아니라고 주장하고 강제로 사용 <span style="color: red">!!사용 자제!!</span>
- \[객체?.\]: safe-calls(안전 호출 연산자) - null인지 확인하고 null이 아닐 때만 참조하도록 함
  - 수신객체가 null이면 null이 나옴 ~\> ${~}에서 참조할 경우 null을 출력
- \[객체?:\]: 엘비스 연산자 - null인 경우 다른 객체로 대체

### 배열
- 배열을 사용하면 위치정보가 연속적인 메모리 공간을 확보, 순서를 매겨 사용할 수 있다.  
cf. 일반적으로 변수 선언 시 Kotlin은 역시 주소 공간에 띄엄띄엄 랜덤으로 변수 공간을 확보한다.  
  - arrayOf() 메서드 사용
- 배열의 요소 하나하나들이 각각 변수라고 생각하면 됨 - index를 통해 접근
- 반복적으로 변수에 접근하는 행위를 할 수 있게 됨  
ex. for((idx, kor) in kors.withIndex()): kors 배열을 withIndex()로 Pair의 Collection으로  
  바로 구조 분해로 받아서 사용하는 듯하다.

### 컬렉션
- 배열과 달리 크기가 정해져있지 않고, 동적으로 값을 추가할 수 있는 자료구조들
  - 읽기전용(immutable) / 수정가능(mutable)로 나뉜다.
- List: 배열과 유사한 느낌, 순서 있음
- Map: Pair(key, value) 로 만든 자료형
- Set: 순서 없음, 중복 없음(여러 번 add해도 같은 자료는 하나만)
  - 요소가 존재하는지에만 집중 =\> 교집합, 차집합, 합집합 함수로 요소 추출 가능
- (참고) Generic(제네릭)
  - Any 타입 대신  
인스턴스에서 사용할 자료형을 인스턴스를 생성할 때 고정(compile time에)
  - 객체 자료형의 안정성 ↑, 형변환 번거로움 ↓

### Single-expression function
- 람다식: 메서드를 간결하게 표현할 수 있는 방법
  - Java 8 이상 버전 Java와 같이 Kotlin도 람다식 지원
- ex. fun add(num: Int, num2:Int, num3:Int) = (num1 + num2 + num3) / 3  
=\> 함수를 람다식으로 정의
- ex. var add = {num: Int, num2: Int, num3: Int -> (num1 + num2 + num3) / 3}  
=\> 메서드를 선언하지 않고도 로직을 저장할 수 있다.

### 싱글턴
(강의 자료에는 있는데 강의가 없음)
- 메모리 전역에서 유일한 객체임을 보장, 위치도 고정
  - 프로그램 실행 시점에 메모리에 로딩
- Kotlin에서는 companion, object 키워드로 싱글턴 구현  
<https://lannstark.tistory.com/141> Java static과 비교, constant 키워드  
<https://velog.io/@yongin01/자바코틀린-companion-object에-대해> 메모리 그림 참고

---

## 5주차: Kotlin 심화
### 유용한 기능
- 타입 변환
  - 숫자 \<-\> 숫자 toXxx() 메서드가 들어있음
  - String \<-\> 숫자 parseInt(~)와 같은 별도 메서드 사용
  - 그 외 타입 \<-\> 타입: 상속 관계에서 가능
    - 업 캐스팅(자식 클래스 => 부모 클래스 타입): as SuperType명 - 생략 가능
    - 다운 캐스팅 - 원래 SubType 객체였어야만 가능함(당연)
- 타입 확인: is 키워드 사용
- 여러 인스턴스를 한 번에 반환하는 것처럼 보이게 하기: Pair, Triple  
// 네 개 이상 인스턴스를 갖는 타입은 따로 정의 필요
- 자기 자신의 객체를 전달 가능(여러 scope function들)
  - let: 중괄호 블록 안에 it으로 넘김
  - with: 중괄호 블록 안에 this로 넘김 - this는 생략 가능하므로, null이 아닐 때만 사용
  - also: 중괄호 블록 안에 it으로 넘김
  - apply: 중괄호 블록 안에 this로 넘김 
  - run
    - 객체에서 호출하지 않는 경우: 잠시 임시 영역을 만들어주는 느낌
    - 객체에서 호출하는 경우: with와 비슷한데 null 체크를 할 수 있어서 더 안전하게 사용 가능

### 확장 함수
- 확장 함수: 기존 클래스에 메서드를 추가하기
  - 원본 클래스를 수정하지 않고 일관성 유지 가능 // 과도하게 사용하면 가독성 떨어지므로 유의

### 비동기 프로그래밍
- 동기 작업: 요청을 보내고 결과값을 받을 때까지 작업을 멈춤
- 비동기 작업: 요청을 보내고 결과값을 받을 때까지 작업을 멈추는 게 아니라 결과값을 받기 전에 다른 일을 수행

### 스레드
- 프로그램에는 하나의 메인 스레드(실행흐름)이 존재 // Kotlin 프로그램에서는 fun main()
  - 별도 자식 스레드를 생성해서 로직을 동시에 실행 가능
  - 코틀린은 thread 키워드로 스레드를 생성
- 프로그램 ==(메모리에 올라감)==\> 프로세스 (메모리 heap 영역)
  - 각 스레드들은 각자의 독립 메모리 stack 영역을 가짐 =\> 스레드 생성 시 스택의 일정 부분 차지
- Kotlin에서 사용하려면 kotlinx.coroutines 외부 종속성 추가

### 코루틴
- 스레드보다 더 경량화된 동시 처리 기능 제공 // OS 지식 없어도 사용 쉬움
  - 로직routine을 협동co해서 실행하는 것
- 코루틴은 빌더와 함께 사용
  - launch는 결과값이 없는 코루틴 빌더
  - async는 결과값이 있는 코루틴 빌더
- 코루틴은 스코프로 범위를 지정: GlobalScope / CoroutineScope
  - 스코프별 특징에 따라 정리 등 해야할 일이 달라짐
- 실행할 스레드를 지정해야 코루틴을 실행할 수 있음
  - Dispatcher를 이용해서 실행할 스레드를 지정
  - 안드로이드 개발의 경우 Dispatcher 간 변환 작업을 고려함
- 원래 main()이 실행된 후 바로 종료되는 환경이라면 코루틴 실행을 제대로 확인할 수 없지만  
job의 join()으로 가능한 환경으로 만들 순 있음
  - main()이 실행된 후 바로 종료되는 환경이라면 코루틴 실행을 제대로 확인할 수 없음  
***Kotlin 공식 문서에서 Coroutine 찾아서 볼 것

### 스레드와 코루틴
- 스레드, 코루틴 모두 동시성 프로그래밍을 위한 기술
- 스레드
  - thread 이용 시 작업 하나하나의 단위가 thread가 됨 =\> 각 thread가 독립적인 stack 메모리 영역을 가짐
  - thread의 동시성 보장 수단 =\> OS kernel의 context switching
    - 이 때 다른 task의 결과가 필요해서 다른 thread를 호출하게 되면 blocking이 발생
- 코루틴
  - coroutine 이용 시 작업 하나하나의 단위가 coroutine object가 됨  
=\> 여러 작업에 각각 object를 할당  
=\> coroutine object도 객체이므로 JVM Heap에 적재됨
  - 동시성 보장 수단 =\> Programmer Switching: 소스코드로 switching 시점을 정함, OS는 관여 x
    - 이 때 다른 task의 결과가 필요해서 다른 coroutine object를 호출하면 suspended(non-bloking)  
=\> 호출한 object와 호출된 object가 동일한 thread에서 실행된다.
    - 하나의 thread를 더 잘게 쪼개서 사용하는 꼴이 되므로 light weight thread라고도 한다.
