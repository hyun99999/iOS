# RxSwift 와 MVVM
<p align="center"><img src ="https://user-images.githubusercontent.com/69136340/105194329-d0acf400-5b7c-11eb-8ab5-6f5f29e7b24a.png" width = "200"></p>

### RxSwift 란?
> 연속성형태와 함수형태의 연산자를 이용해서 비동기&이벤트를 위한 코드로 구성하고 있는 라이브러리이다.
  - MVVM 모델에서 View 와 View Model 을 바인딩하기 위한 효과적인 도구이다. Delegate 나 KVO, Notification, completion block closure 를 사용할 수 있지만 RxSwift 는 습득이 쉽다는 장점이 있다.
- RxSwift 에서는 모든 것이 스트림이며 UI이벤트나 네트워크 요청도 역시 스트림이다. 
- 변화할 수 있는 상태를 쉽게 대처 할 수 있고, 이벤트들의 순서의 구성, 코드분리, 재사용성을 향상 가능.

### Rx 의 3요소(Oservables, Operators, ???)
- 예를들어 우리가 항상 핸드폰을 예의주시하고 있는 것을 RxSwift 에서는 Subscribe 라고 한다. 우리 바로 구독자 Subscriber, Observer 이다. 핸드폰은 Observalble 이락 한다. 우리는 핸드폰을 구독하고 있다가 방출하는 이벤트에 반응하는 것이다. 
- Rx 에서는 Observable 과 Observers 가 존재. ObservableType protocol 을 준수한다면 어떠 것도 Observable 이 될 수 있다.
- Rx 에서는 모든 함수 연산자로 칭한다.
- 스트림은 next, completed, eroor 3가지 종류의 이벤트로 구성되어 있다.
  - Observable 이 구독자에게 방출할 값이 있다면 next 이벤트를 통해 방출합니다. 이렇게 방출하닥 더이상 방출할 값이 없다면 마지막으로 completed 이벤트를 방출하고 종료됩니다. 이 과정을 스트림이라 볼 수 있다.
  - next :  최신/다음 데이터를 '전달'하는 이벤트
  - completed : 성공적으로 일련의 이벤트들을 종료시키는 이벤트. 즉, Observable가 성공적을 자신으 ㅣ생명주기를 완료했으며, 추가적을 이벤트를 생성하지 않는다.
  - error : Observable 이 에러를 발생, 추가적을 이벤트를 실행하지 않으 것임을 의미(에러와 함께 완저 종료)
 ```swift
  var observableInt = Observable.of(1, 2, 3, 4, 5, 6)
  observableInt.subscribe { print($0) }
// next(1)
// next(2)
// next(3)
// next(4)
// next(5)
// next(6)
// completed
 ```
- 구독할 수 있듯 취소할 수도 있다. 이것을 DisposeBag 에 담아 구독을 취소하는 과정을 거치게 된다.(순환 참조로인한 Memory Leak 을 피하기 위해서)
```swift
let disposeBag = DisposeBag()

var observableInt = Observable.of(1, 2, 3, 4, 5, 6)
observableInt
  .subscribe { print($0) }
	  .disposed(by: disposeBag)
 ```
  - 그러면 ViewController 가 메모리에서 해제될 때 disposeBag 에 담긴 것 메모리에서 같이 해제. 만약 중간에 해제를 원한다면 disposeBag  을 초기화시켜주며 된다.
   ```swift
   disposeBag = DisposeBag()
   ```
1. Observable
- Finite Observable Sequences

- Infinite Observable Sequences
	- 보통 UI 이벤트는 무한하 관찰가능한 sequence 이다. 예로들어 기기으 가로/세로 모드에 따라 변해야하는 코드를 생각해보면 사용자가 디바이스를 절대 회전시키 않는닥 해서 이벤트가 종료되 것이 아니다. 단지 발생하지 않았을뿐. 

2. Operators
- ObservableType과 Observable 클래스에는 복잡한 논리를 구현하기 위해 많은 메서드가 포함되어 있다. 이 메서드들을 Operator라고 부른다.
- Operator는 비동기 입력을 받아 출력만 생성하기 때문에 Operator들끼리 쉽게 혼합해서 사용이 가능합니다. Rx Operatros들은 Observable의 의해 들어온 값들을 처리하고 최종값이 나올때 방출합니다.

3. Schedulers
- Schedulers 는 Rx 에서 dispatch queue 와 동일. 

### Reactive Programming
- RxSwift 는 Reactive Programming 에 기반을 두고 있다. 
- Reactive. 반응형 즉 변화에 반응하여 자동으로 변경되는 것이 핵심.
```swift
//Reactice Programming
var a = 3
var b = 7
var c = a + b
a = 1
print(c) // 8
```
### MVVM 과 밀접한 연관
  - 데이터 바인딩을 제공하는 플렛폼에서 만들어진 이벤ㅌ 중심 프로그램을 윟 특별히 개발된  RxSwift 와 연관성이 높다.

- 출처 : https://ios-development.tistory.com/95
- 출처 : https://jinshine.github.io/2019/01/01/RxSwift/1.RxSwift란/

### 왜 RxSwift 인가?
