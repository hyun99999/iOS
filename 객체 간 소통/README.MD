# 객체 간 소통
- Async 환경에서(비동기처리) 스위프트는 객체 간 소통을 3가지 방법으로 할 수 있다.
  > Async 란 다른 작업을 기다리거나 방해하지 않고 바로 자신의 작업을 진행하는 프로그래밍이다. 
- callback, notification, delegation

1. Callback 을 통해 인지
- 비동기 작업이 완료되는 시점을 Completion Handler 의 Callback Closure 를 통해 인지.
  > 콜백 클로저란 다른 함수의 인자로 들어간 클로저를 말한다. 함수의 인자로 클로저가 들어가면 함수는 코드내에서 언제든지 클로저를 부를 수 있다. 이런 구조를 활용해서 함수가 자신의 작업이 모두 완료된 후 클로저를 호출하여 비동기작업이 완료된 시점을 인식하고 처리 가능하도록 한다.
```swift
func someFunction(someClosure: () -> ()) {
  //some codes
  someClosure()
}
```

- Escaping Closure 를 통해 Completion 이벤트를 처리하게끔 하는 코드의 예시를 살펴보자.
  > Escaping Closure
    >> - 함수의 전달 인자로 전달한 클로저가 함수 종료 후에 호출될 때 클로저가 함수를 탈출한다고 표현. **@escaping** 키워드를 통해서 탈출 허용 명시.
    >> - 비동기 작업으로 함수가 종료되고 난 후 호출할 필요가 있는 클로저를 사용해야할 때 **Escaping Closure(탈출 클로저)**가 필요.
    >> - 함수가 탈출할 수 있는 경우에는 함수 외부에 정의된 변수나 상수에 저장되어 함수가 종료된 후에 사용할 경우가 있다.
    >> - 클로저가 함수 외부로 전달되어 외부에서 사용이 가능하거나, 외부 변수, 상수에 저장되는 등 클로저가 탈출할 수 있는 조건이 명확하다면 @escaping 키워드를 꼭 명시해야한다.
    >> - 탈출클로저임을 명시한 경우, 클로저 내부에서 해당 타입의 프로퍼티나 메서드, 서브스크립트 등에 접근하려면 self 키워드를 명시적으로 사용해야 한다.
```swift
//1.탈출한 클로저를 저장할 Array. 즉, 외부에 정의된 변수에 클로저가 저장되는 경우.
var completionHandlers: [() -> void] = []

func someFunctionWithEscapingClosure(completionHandler: @escaping () -> void) {
  completionHandlers.append(completionHandler)
}

//2.함수에서 반환한 클로저가 함수 외부의 상수에 저장되는 경우.
typealias VoidVoidClosure = ()-> void

let firstClosure: VoidVoidClosure{
  print("Closure A")
}

//탈출클로저를 매개변수로 가진 함수에서 탈출클로저 리턴.
func returnClosure:(first: @escaping VoidVoidClosure) -> VoidVoidClosure{
  return first
}

//상수에 저장.
let returnedClosure : VoidVoidClosure = returnClosure(first : firstClosure)

returnedClosrue() //Cosure A
```

2. Notification 을 통해 인지

3. Delegation 을 통해 인지
- Delegation 도 Notification 처럼 한 객체가 다른 객체에 말을 걸기 위한 방법. MVC 구조에서 TableView 나 ScrollView 또는 UIView와 같은 View 가 Controller 에게 말을 걸기 위해 사용되는 방식.

(1) Delegation Protocol
- MVC 패턴에서 View 는 데이터를 다루는 부분과 독립적이어야 하기 때문에 데이터를 가져오거나 보내는 동작을 수행할 때는 Controller 에게 이를 위임(Delegation) 하게 된다.
- iOS SWIFT 에서 Delegation Protocol 이 가장 많이 활용되는 것은 TableView 이다. View 로써 테이블 안에 몇개의 데이터가 존재하는지. 어떤 데이터가 존재하는지. 유저가 테이블의 행을 눌렀을 때 어떻게 해야하는지 등을 알 수 없고 제어도 할 수 없다. 이러한 역할을 Controller 에게 delegate 이나 datasource 와 같은 Delegation Protocol 로써 위임하게 된다.

(2) Delegation Protocol 만들기
> 1. Delegation Protocol 정의
> 2. View 에 delegate property 추가
> 3. View 에 Delegation Protocol 에서 정의한 메서드를 통해 delegate 으로 부터 받은 정보를 활용하는 코드 추가.
> 4. Controller 가 Delegation Protocol 을 따른다고 선언.
> 5. Controller 에 Delegation Protocol 이 요구하는 메서드 추가.
> 6. Controller 에 View 의 인스턴스를 만든 다음, View 인스턴스의 delegate property 로 self 대입.
```swift
//1)Delegate protocol
protocol MyProtocol {
  func protocolFunc() -> Int
}

class View {
  //2)View 에 delegate property 추가
  var delegate: MyProtocol?
  
  //3)View 에 delegate 한테 받은 정보를 활용하는 코드 추가.
  fucn tapped() {
    if let delegate = delegate {
      let number = delegate.protocolFunc()
      print(number)
    }
  }
}

//4)Controller 가 Delegation Protocol 을 따른다고 선언.
class Controller: MyProtocol {
  var myView: View?
  var controllerInstanceProperty = 10
  
  //5)Protocol이 강제하는 API 작성
  func protocolFunc() -> Int{
    return 2
  }
  
  init() {
    //6)Controller 를 View 의 delegate 으로 지정.
    myView = View()
    myView!.delegate = self
    myView?.tapped()
  }
}

var myController = Controller()
//print 2

myView.delegate.controllerInstanceProperty
//에러 : delegate 로 myController class 의 API 전부 들고온 것이 아니라 protocol 에서 정의한 것만 가져오기 때문.
```


- 출처 : https://m.blog.naver.com/PostView.nhn?blogId=jdub7138&logNo=220937372865&proxyReferer=https:%2F%2Fwww.google.com%2F
- 출처 : SWIFT 스위프트 프로그래밍 3판 / 야곰 지음
