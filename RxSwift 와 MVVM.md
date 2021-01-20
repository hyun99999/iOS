# RxSwift 와 MVVM
<p align="center"><img src ="https://user-images.githubusercontent.com/69136340/105194329-d0acf400-5b7c-11eb-8ab5-6f5f29e7b24a.png" width = "200"></p>

### RxSwift 란?
> 연속성형태와 함수형태의 연산자를 이용해서 비동기&이벤트를 위한 코드로 구성하고 있는 라이브러리이다.
  - MVVM 모델에서 View 와 View Model 을 바인딩하기 위한 효과적인 도구이다. Delegate 나 KVO, Notification, completion block closure 를 사용할 수 있지만 RxSwift 는 습득이 쉽다는 장점이 있다.

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
https://usinuniverse.bitbucket.io/blog/rxswiftmvvmpart1.html

- 왜 RxSwift 인가?
https://ios-development.tistory.com/95
