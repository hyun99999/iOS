# iOS
SWIFT 스위프트 프로그래밍 3판 swift5 /야곰 지음
을 읽고 공부한 내용을 정리 중

:umbrella: iOS 공부


1/1(금)

ch9.구조체와 클래스
- 구조체
- 클래스

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
      > - @escaping 통해서 비동기 작업으로 함수 종료되고 난 후 호출할 필요있는 클로저를 사용해야할 때.
      > - 함수 외부에 정의된 변수나 상수에 저장되어 함수가 종료된 후 사용할 수 있다.
    - auto closure
      > - 자동 클로저는 전달인자를 갖지 않는다. @autoclosure 기본적을 비탈출 클로저. 
      > - 함수의 전달인자로 전달하는 표현을 자동으로 변환해주는 클로저.
 
 ch14.옵셔널 체이닝과 빠른 종료
  - 옵셔널 체이닝
    > - 옵셔널에 속해 있는 nil 일지도 모르는 프로퍼티, 메서드, 서브스크립션 등을 가져오거나 호출할 때 사용할 수 있는 일련의 과정.
    > - 한 단계뿐만 아니라 여러 단계로 복잡하게 중첩된 옵셔널 프로퍼티나 메서드 등에 매번 nil 체크를 하지 않아도 손쉽게 접근 가능. 할당도 가능.
    > - 옵셔널 바이닝을 통해 결과값이 nil 이 아님을 확인하는 동시에 값을 받아올 수 있음.
  - 빠른 종료
    ```swift
    guard ... else {}
    ```
    > - 특정 조건에 부합하지 않다는 판단이 되면 빠르게 코드 블록의 실행을 종료 가능.
    > - 코드 종료할 때는 
    ```swift
    return, break, continue, throw 
    ```
    > 등의 제어문 전환 명령을 사용.

ch15.맵,필터,리듀스
  - Map맵
    > - 자신을 호출할 때 매개변수로 전달된 함수를 실행하여 그 결과를 다시 반해주는 함수.
    > - 컨테이너가 담고 있던 각각의 값을 매개변수를 통해 받은 함수에 적용한 후 다시 컨테이너에 포장하여 반환. 이때 새로운 컨테이너가 생성되어 반환.
    > - 클로저 표현식을 사용해서 간략화 가능. 배열, 딕셔너리, 세트, 옵셔널 등에서 사용 가능.
  - Filter필터
    > - 컨테이너 내부의 값을 걸러서 추출하는 역할. 맵과 마찬가지로 새로운 컨테이너에 값을 담아 반환.
    > - 맵과 필터를 체인처럼 연결하여 사용 가능.
  - Reduce리듀스
    > - 컨테이너 내부의 콘텐츠를 하나로 합하는 기능을 실행하는 고차함수.
    > - 첫번째 형태는 클로저가 각 요소를 전달 받아 연산한 후 값을 다음 클로저 실행을 위해 반환하며 컨테이너를 순환하는 형태.
    > - 두번재 형태는 클로저가 따로 결과값을 반환하지 않는 형태. 대신 inout 매개변수를 사용하여 초기값에 직접 연산을 실행.

ch16.모나드
  - 컨텍스트
    > 옵셔널이 여기 해당. 컨텐츠를 담고있다.
  - 함수객체
    > 맵을 적용할 수 있는 컨테이너 타입. Array, Dictionary, Set 등.
  - 모나드
    - flatMap
      > Map 과 다르게 컨텍스트 내부의 컨텍스트를 모두 같은 위상으로 평평하게 펼쳐준다는 의미.
    ```swift
    func map<U>(_ transform: (Wrapped) throws -> U) rethrows -> U?
    func flatMap<U>(_ tranform: (Wrapped) throws -> U?) rethrows -> U?
    ```
      > - flatMap 은 클로저를 실행하면 알아서 내부 컨테이너까지 값을 추출.
      > - flatMap 은 .none 이되거나 nil 이 되는 등에는 별도의 예외처리없이 빈 컨테이너를 반환.

ch17.서브스크립트 문법
  > - 인스턴스의 이름 뒤에 대괄호로 감싼 값으 써줌으로써 인스턴스 내부의 특정값에 접근 가능.
  > - 하나의 타입이 여러개의 서브스크립트를 가질 수도 있다. 
  ```swift
  //서브스크립트 정의 문법
  subscript(... : ...) -> ... {}
  //사용
  instance[]
  ```
  - 타입 서브스크립트
    > 인스턴스가 아닌 타입 자체에서 사용할 수 있는 서브스크립트. subscript 키워드 앞에 static 키워드 추가. 클래스의 경우 class 키워드 사용가능(상속 시 재정의 가능.)

ch18.상속
  - 상속
  - 재정의 override
    - 메서드 재정의
    - 프로퍼티 재정의
      > - 프로퍼티를 재정의한다는 것은 프로퍼티 자체가 아니라 프로퍼티의 접근자, 설정자, 프로퍼티 감시자 등을 재정의하는 것.
      > - 읽기 전용 프로퍼티였더라도 자식클래스에서 읽기쓰기 가능한 프로퍼티로 재정의 가능. 읽기쓰기 가능한 프로퍼티는 읽기 전용으로 재정의 불가.
      > - 프로퍼티 감시자를 재정의하더라도 조상클래스에 정의한 프로퍼티 감시자도 동작.
      > - 재방지
      ```swift
      final ~
      ```
  - 클래스의 이니셜라이저
    - 지정 이니셜라이저(designated initializer)
      > 이니셜라이저가 정의된 클래스의 모든 프로퍼티를 초기화해야 하는 의무가짐. 
      ```swift
      init(매개변수들) {
        초기화구문
        }
      ```
    - 편의 이니셜라이저(convenience initializer)
      > 지정 이니셜라이저를 자신 내부에서 호출한다.
      ```swift
      convenience init(매개변수들) {
      초기화구문
      }
      ```
    -  2단계 초기화를 위해서 네가지 안전확인을 진행한다.
      > 1. 자식 클래스의 지정 이니셜라이저가 부모클래스의 이니셜라이저를 호출하기 전에 자신의 프로퍼티를 모두 초기화했는지 확인.
      > 2. 자식클래스의 지정 이니셜라이저는 상속받은 프로퍼티에 값을 할당하기 전에 반드시 부모클래스의 이니셜라이저를 호출.
      > 3. 편의 이니셜라이저는 자신의 클래스에 정의한 프로퍼티를 포함하여 그 어떤 프로퍼티라도 값을 할당하기 전에 다른 이니셜라이저를 호출해야 합니다.
      > 4. 초기화 1단계를 마치기 전까지는(상속 체인을 따라 최상위 클래스까지의 모든 저장 프로퍼티에 값이 있다고 확인하는 것) 이니셜라이저는 인스턴스 메서드를 호출할 수 없다. 또 , 인스턴스 프로퍼티의 값을 읽어들일 수도 없다. self 프로퍼티를 자신의 인스턴스를 나타내는 값으로 활용할 수도 없다.
    - 이니셜라이저 상속 및 재정의
      - 자식클래스의 편의 이니셜라이저가 부모클래스의 지정 이니셜라이저를 재정의하는 경우 override 를 붙인다.
      - 자식클래스에서 부모클래스의 편의 이니셜라이저는 절대로 호출 할 수 없기때문에 이때는 override 붙이지 않는다.
    - 이니셜라이저 자동 상속
      - 특정 조건에서 부모의 이니셜라이저가 자동으로 상속된다. 사실 기본적으로 상속받은 이니셜라이저는 자식클래스에 최적화되어 있지 않아서 스위프트에서는 상속받지 않는다. 하지만 대부분의 경우 자식클래스에서 이니셜라이저를 재정의해줄 필요가 없다.
      - 자식클래스에서 프로퍼티 기본값을 모두 제공한다고 가정할때.
        - 규칙1 : 자식클래스에서 별도의 지정 이니셜라이저를 구현하지 않는다면 부모클래스의 지정 이니셜라이저가 자동으로 상속.
        - 규칙2 : 만약 규칙1 에 따라  자식클래스에서 부모클래스의 지정 이니셜라이저를 자동으로 상속받은 경우 또는 부모클래스의 지정 이니셜라이저를 모두 재정의하여 부모클래스와 동일한 지정 이니셜라이저를 모두 사용할 수 있는 상황이라면 부모클래스의 편의 이니셜라이저가 모두 자동 상속.
    - 요구 이니셜라이저
      ```swift
      required init() {}
       ```
       > - 클래스의 이니셜라이저 앞에 명시해주면 상속받는 자식클래스에서 반드시 해당 이니셜라이저를 구현해주어야 합니다. 하지만 자동 상속 가능.
       > - 재정의됨과 동시에. 편의 이니셜라이저임과 동시에 요구 이니셜라이저도 가능. required 만 앞에 붙이면 된다.

ch19.타입캐스팅
  - 스프트의 타입캐스팅은 인스턴스의 타입을 확인하거나 자신을 다른 타입의 인스턴스인양 행세할 수 있는 방법으로 사용.
  - 'is', 'as' 연산자로 값의 타입을 확인하거나 다른 타입으로 전환 가능.
  - 데이터 타입 확인 (is)
    - 어떤 인스턴스가 어떤 클래스의 인스턴스인지 타입을 확인 가능. 모든 데이터 타입에 사용 가능.
  - 다운캐스팅
    - 부모클래스의 타입을 자식클래스의 타입으로 캐스팅.
    - as? : 다운캐스팅이 실패했을 경우 nil 반환. 반환타입이 옵셔널.
    - as! : 다운캐스팅이 실패했을 경우 런타임 오류 발생. 반환타입이 옵셔널 x. 성공했을 경우 인스턴스가 반환.
  - Any,AnyObject
    - Any : 함수 타입을 포함한 모든 타입을 뜻함
    - AnyObject : 클래스 타입만을 뜻함

ch20. 프로토콜
  > - 프로토콜은 특정역할을 하기 위한 메서드, 프로퍼티, 기타 요구사항 등의 청사진. 프로토콜의 요구사항을 모두 따르는 타입은 '해당 프로토콜을 준수한다(conform)'고 표현.
  > - 즉, 프로토콜은 정의를 하고 제시를 할 뿐이지 스스로 기능을 구현하지 않는다.
  ```swift
  protocol 프로토콜이름 {
  프로토콜 정의
  }
  //프로토콜 채택
  class SomeClass : AProtocol,BProtocol {}
  ```
  - 프로토콜 요구사항
    - 프로퍼티 요구
      > - 프로토콜을 채택한 타입은 요구하는 프로퍼티의 이름과 타입만 맞도록 구현하면 됨. 다만 읽기전용, 읽고쓰기는 프로토콜이 정해야한다.
      > - 프로퍼티 요구사항은 항상 var 키워드를 사용한 변수 프로퍼티로 정의.
      ```swift
      protocol SomeProtocol {
        //읽고쓰기가능
        var settableProperty: String { get set }
        //읽기전용
        var notNeedToBeSettableProperty: String { get }
      }
      ```
      > 타입프로퍼티를 요구하려면 'static' 키워드를 변수앞에 사용합니다. 상속 가능한 타입 프로퍼타입인 class 과 static 을 구분하지 않고 모두 static 사용
    - 메서드 요구
      > - 프로토콜이 요구할 메서드는 포로토콜 정의에서 작성. 실제 구현부인 {} 중괄호 부분은 제외하고 메서드의 이름, 매개변수, 반환 타입 등만 작성.
      > - 타입 메서드를 요구할 때는 앞에 'static' 키워드를 명시.  
    - 가변 메서드 요구
      > - 메서드가 인스턴스 내부의 값을 변경할 필요가 있다. (값 타입의 인스턴스 메서드에서 mutating 사용해서 값을 변경했었다.)
      > - 참조 타입인 클래스의 메서드 앞에는 mutating 키워드를 명시하지 않아도 문제가 없지만 값 타입인 구조체와 열거형의 메서드 앞에는 'mutating' 키워드를 붙인 가변 메서드 요구가 필요.(즉, 클래스구현에서는 'mutating' 키워드를 써주지 않아도 됩니다.)
    - 이니셜라이저 요구
      > - 메서드 요구와 마찬가지로 이니셜라이저를 정의하지만 구현은 하지 않습니다.
      > - 구조체는 상속할 수 없기 때문에 이니셜라이저 요구에 대해 신경쓸 필요가 없지만 클래스의 경우라면 다르다.
      > - 'required' 식별자를 붙인 요구 이니셜라이저로 구현.
      > - 부모클래스가 프로토콜을 준수한다면 자식클래스도 요구에 부합하는 이니셜라이저를 구현해야한다.
        > 부모클래스에서 요구 이니셜라이저를 구현하고 자식클래스가 또 프로토콜을 준수한다면 'override' 와 'required' 를명시하여 구현.
      > - 일반 이니셜라이저 외에도 실패 가능한 이니셜라이저를 요구할 수도 있다. 이때 해당 이니셜라이저를 구현할 때 실패가능한( init?() ) 이니셜라이저로 구현해도, 일반적인 이니셜라이저로 구현해도 무방.
    - 프로토콜의 상속과 클래스 전용 프로토콜
      > - 프로토콜은 하나 이상의 프로토콜 상속을 받아 기존 프로토콜보다 더 많은 요구사항을 추가할 수 있다. 상속과 동일하게 표기.
      > - 클래스 전용 프로토콜로 제한을 주기 위해서는 프로토콜의 상속리스트의 맨 처음에 class 키워드가 위치해야 한다.
    - 프로토콜 조합과 프로토콜 준수 확인
      > - 하나의 매개변수가 여러 프로토콜을 모두 준수하는 타입이어야 한다면 하나의 매개변수에 '&' 사용해서 여러 프로토콜을 한 번에 조합하여 요구 가능.
      > - 'is' 와 'as' 연산자를 통해 프로토콜을 준수하는지 확인가능하고, 특정 프로토콜로 캐스팅 가능.
    - 프로토콜의 선택적 요구
      > - 프로토콜의 요구사항 중 일부를 선택적 요구사항으로 지정 가능.
      > - 이때 objc 속성이 부여된 프로토콜만 가능.(objc 사용하기위해서 Foundation 프레임워크 모듈을 임포트해주어야 한다.)
      > - 해당 요구사항의 타입은 자동적으로 옵셔널이 된다. 함수 자체가 옵셔널이 된다.
      > - 옵셔널 체이닝을 통해 선택적 요구사항을 호출 가능.
    - 프로토콜의 변수와 상수
      > - 프로토콜 이름만으로 스스로 인스턴스를 생성하고 초기화할 수 없지만 트정 프로토콜을 준수하는 타입의 인스턴스를 할당 가능.

ch21.익스텐션
  > - 구조체, 클래스, 열거형, 프로토콜 타입의 새로운 기능을 추가 가능. 새로우 기능을 추가할 수 있지만 기존에 존재하는 기능을 재정의할 수는 없다.
    > 익스텐션을 사용하는 대신 원래 타입으 정의한 소스에 기능을 추가하는 방법도 있겠지만, **외부 라이브러리나 프레임워크를 가져다 사용했다면 원본 소스를 수정하지 못한다.** 이때 원하는 기능을 추가하고자 할 때 사용.
  > - 상속은 수직 확장. 익스텐션은 수평 확장. 상속은 기존 기능을 재정의 가능. 익스텐션은 재정의 불가.
  ```swift
  extension 확장할 타입 이름 {
    //타입에 추가될 새로우 기느 구현
  }
  //익스텐션은 추가로 다른 프로토콜을 채택할 수 있도록 할 수 있다.
  extension 확장할 타입 이름 : 프로토콜1, 프로토콜2 {
  }
  ```
  - 연산 프로퍼티 추가
    > 익스텐션으로 연산 프로퍼티를 추가 가능. 저장 프로퍼티 추가 불가. 기존의 프로퍼티에 프로퍼티 감시자 추가 불가
  - 메서드 추가
    > 인스턴스, 가변, 타입 메서드 추가 가능. 여러 익스텐션 블록으로 나눠서 구현 가능. 
  - 이니셜라이저 추가
    > 클래스 타입에 편의 이니셜라이저는 추가할 수 있지만, 지정 이니셜라이저는 추가 불가. 
  - 서브스크립트 추가
  - 중첩 데이터 타입 추가(ch24에서 자세히)

ch22.제네릭
  > 제네릭으로 구현한 기능과 타입은 재사용하기도 쉽고, 코드의 중복을 줄일 수 있다.
  ```swift
  제네릭을 사용하고자 하는 타입 이름 <제네릭을 위한 타입 매개변수>
  ```
  - 제네릭 함수 
  > -  실제 타입 이름 (Int, String 등) 을 써주는 대신 플레이스홀더(Placeholder) 보통 T 자주사용. 혹은 카멜케이스 사용하여 표현.
    > -  플레이스홀더는 타입의 종류를 알려주지 않지만 말 그대로 어떤 타입이라는 것을 알려줌. 실제 타입은 함수가 호출되는 순간 결정,
    > 즉, 어떤 타입이든 상관없이 수용할 수 있도록 구현하려고 했다면 Any 를 사용해주면 그만이지만, 우리는 같은 타입에 대해서 수용하지만 어떤 타입이든 상관없다라는 상황에서 사용.
  - 제네릭 타입
    > 플레이스홀더를 사용해서 제네릭 타입을 만들면 타입 선언 시 그 타입에만 동작할 수 있도록 제한할 수 있어 안전하고 의도한 대로 기능을 사용하도록 유도 가능.
  - 제네릭 타입 확장(extension)
    > 익스텐션을 통해 제네릭을 사용하는 타입에 기능을 추가하고자 한다면 **익스텐션 정의에 타입 매개변수를 명시하지 않아야 합니다.** 원래의 제네릭 정의에 명시한 타입 매개변수를 익스텐션에서 사용할 수 있다.
  - 타입 제약(Type Constraints)
    > 제네릭 함수가 처리해야할 기능이 특정 타입에 한정되어야만 처리할 수 있다던가. 제니릭 타입을 특정 프로토콜을 따르는 타입만 사용할 수 있도록 제약을 둬야하는 상황 발생할떄 사용.
    > - 타입제약은 타입 매개변수가 가져야 할 제약사항을 지정할 수 있는 방법.
    > - 타입제약은 클래스타입 또는 프로토콜만 줄 수 있음.
    > - 제약을 주고싶으면 타입 매개변수 뒤에 콜론을 붙인 후 원하는 클래스 타입 또는 프로토콜을 명시.
    > - 여러 제약을 추가하고 싶다면 콤마로 구분해주는 것x. where 사용.
    ```swift
    //T는 BinaryInteger 프로토콜 준수하고, FloatingPoint 프로토콜도 준수하는 타입만 사용할 수 있다.
    func swapTwoValues<T: BinaryInteger>(_ a: inout T, _ b: inout T) where T : FloatingPoint {}
    ```
  - 프로토콜의 연관 타입(Associated Type)
    > - 연관타입은 프로토콜에서 사용할 수 있는 플레이스홀더 이름.
        > 타입 매개변수의 그 역할을 프로토콜에서 수행할 수 있도록 함.
        > 프로토콜 내에서 존재하지 않는 타입으로 연관타입을 정의하여 프로토콜 정의내에서 활용. **어떤 것이어도 상관없지만, 하나의 타입임은 분명하다** 라는 의미.
    > - 프로토콜을 준수할 떄 연관타입 대신에 실제 타입으로 구현해준다. 계속 일관성있게 구현하면 됨.
        > 만약 어떤 타입으로 사용할지 명확히 해주고 싶다면 구현부에서 지정 가능.
    ```swift
    typealias 타입매개변수이름 = 타입
    ```
  - 제네릭 서브스크립트
    > 제네릭 함수를 구현할 수 있었던 것처럼 서브스크립트도 구현 가능. 타입제약도 가능.
    
ch23.프로토콜 지향 프로그래밍
  > 프로토콜, 익스텐션, 제네릭의 조화
	- 프로토콜 초기구현
    > 프로토콜의 요구사항을 익스텐션을 통해 구현하는 것.
    > 특정 프로토콜을 준수하는 타입에 프로토콜의 요구사항을 찾아보고 이미 구현되어 있다면 호출하고 아니면 초기구현의 기능을 호출.
		> - 스위프트의 클래스는 다중상속을 지원하지 않으므로 부모클래스의 기능으로 부족하다면 자식클래스를 다시 구현해야 하지만 **프로토콜 초기구현**을 한 프로토콜을 채택했다면 상속도 추가 구현도 필요 없다.
		> - 상속을 지원하지 않는 값 타입인 구조체, 열거형도 초기구현을 한 프로토콜을 채택한다면 추가 가능.
	- 기본 타입 확장
		> 스위프트의 기본 타입을 확장하여 내 원하는 기능을 공통적으로 추가 가능. 실제 구현 코드를 보고 수정할 수 없기 때문에 익스텐션, 프로토콜, 프로토콜의 초기구현을 사용해 기능 추가 가능. 
