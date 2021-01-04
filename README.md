# iOS
SWIFT 스위프트 프로그래밍 3판 swift5 /야곰 지음
을 읽고 공부한 내용을 정리 중

:umbrella: iOS 공부


1/1(금)

ch9.구조체와 클래스
- 구조체
- 클래스


1/2(토)

ch10.프로퍼티와 메서드
- 프로퍼티
  - 저장 프로퍼티
  - 지연 저장 프로퍼티
  - 프로퍼티 감시자
  - 연산 프로퍼티
  - 타입 프로퍼티
    - 저장 타입 프로퍼티
    - 연산 타입 프로퍼티
- 메서드
  - 인스턴스 메서드
    - mutating
    - self 프로퍼티
  - 타입 메서드
  
ch11. 인스턴스 생성 및 소멸
  - 이니셜라이저
  - 옵셔널 프로퍼티 타입
  - 기본 이니셜라이저와 멤버와이즈 이니셜라이저(클래스x)
  - 초기화 위임
  - failable initializer
  - deinitializer(클래스o)

ch12. 접근제어
  - open
  - public
  - internal
  - fileprivate
  - private

1/3(일)

ch13.클로저
> 장점은 간단한 표현. 함수는 클로저의 일종. 기본적을 비탈출 클로저
  - 기본 클로저
  - 후행 클로저
    > 기본 클로저를 조금 더 읽기 쉽게 바꾼 것. 함수나 메서드의 소괄호를 닫은 후 작성해도 됨.
  - 클로저 표현 간소화
    - 문맥을 이용한 타입 유추
      > 이미 적합한 타입을 준수하고있다고 유추 -> 클로저의 매개변수 타입과 반환 타입을 생략하여 표혀 가능.
    - 단축 인자 이름
      > 첫 번째 전달인자부터 $0,$1,$2 ... 순서로 표현. 키워드 in 사용필요x
    - 암시적 반환 표현
      > return 생략.
    - 연산자 함수
      > 연산자 함수를 클로저의 함수 사용
    - 탈출 클로저
      > @escaping 통해서 비동기 작업으로 함수 종료되고 난 후 호출할 필요있는 클로저를 사용해야할 때.
      > 함수 외부에 정의된 변수나 상수에 저장되어 함수가 종료된 후 사용할 수 있다.
    - auto closure
      > 자동 클로저는 전달인자를 갖지 않는다. @autoclosure 기본적을 비탈출 클로저.
      > 함수의 전달인자로 전달하는 표현을 자동으로 변환해주는 클로저.
 
 ch14.옵셔널 체이닝과 빠른 종료
  - 옵셔널 체이닝
    > 옵셔널에 속해 있는 nil 일지도 모르는 프로퍼티, 메서드, 서브스크립션 등을 가져오거나 호출할 때 사용할 수 있는 일련의 과정.
    > 한 단계뿐만 아니라 여러 단계로 복잡하게 중첩된 옵셔널 프로퍼티나 메서드 등에 매번 nil 체크를 하지 않아도 손쉽게 접근 가능. 할당도 가능.
    > 옵셔널 바이닝을 통해 결과값이 nil 이 아님을 확인하는 동시에 값을 받아올 수 있음.
  - 빠른 종료
    ```swift
    guard ... else {}
    ```
    > 특정 조건에 부합하지 않다는 판단이 되면 빠르게 코드 블록의 실행을 종료 가능.
    > 코드 종료할 때는 
    ```swift
    return, break, continue, throw 
    ```
    > 등의 제어문 전환 명령을 사용.

1/4(월)

ch15.맵,필터,리듀스
  - Map맵
    > 자신을 호출할 때 매개변수로 전달된 함수를 실행하여 그 결과를 다시 반해주는 함수.
    > 컨테이너가 담고 있던 각각의 값을 매개변수를 통해 받은 함수에 적용한 후 다시 컨테이너에 포장하여 반환. 이때 새로운 컨테이너가 생성되어 반환.
    > 클로저 표현식을 사용해서 간략화 가능. 배열, 딕셔너리, 세트, 옵셔널 등에서 사용 가능.
  - Filter필터
    > 컨테이너 내부의 값을 걸러서 추출하는 역할. 맵과 마찬가지로 새로운 컨테이너에 값을 담아 반환.
    > 맵과 필터를 체인처럼 연결하여 사용 가능.
  - Reduce리듀스
    > 컨테이너 내부의 콘텐츠를 하나로 합하는 기능을 실행하는 고차함수.
    > 첫번째 형태는 클로저가 각 요소를 전달 받아 연산한 후 값을 다음 클로저 실행을 위해 반환하며 컨테이너를 순환하는 형태.
    > 두번재 형태는 클로저가 따로 결과값을 반환하지 않는 형태. 대신 inout 매개변수를 사용하여 초기값에 직접 연산을 실행.

ch16.모나드










 
