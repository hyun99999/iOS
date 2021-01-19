# Design Pattern
- 디자인 패턴을 정하게 되면 모든 클래스와 속성을 구조적으로 정리가능하며 팀 작업 시 원활한 의사소통과 코드 수정이 가능.

### MVC(Model-View-Controller)
> - Model : 데이터에 관한 로직 담당(데이터 값 변경 및 관리)
> - View : 사용자에게 보여지는 화면 담당(UI)
> - Controller : Model 과 View 연결(Model 값을 View 에 보여줌)
- 오리지날 MVC 패턴은 iOS 개발에 적합하지 않아(Model,View,Controller 가 너무 밀접하게 연관) 애플에서는 CocoaMVC 패턴을 제시했다.
  - Controller 가 View 와 Model 의 중재자 역할을 하여 View 와 Model 에 독립성 부여.
<img src = "https://user-images.githubusercontent.com/69136340/104989633-3a82ac00-5a5e-11eb-99e6-55d137cca460.PNG" width="600">
  
- 하지만 애플의 CocoaMVC 패턴에서는 View 와 Controller 의 역할 분리가 완벽하지 않다.
  - Controller 역할을 수행하는 UIViewController 은 Controller 가 View 를 포함하는 것은 물론 View 의 life-cycle 까지 관리하기 때문에 무거워지고, 분리가 어렵고, 재사용도 어렵고, 각각 테스트하기도 어렵다.
  - 그래서 실제로는 아래와 같은 구조가 된다.
<img src = "https://user-images.githubusercontent.com/69136340/104989638-3b1b4280-5a5e-11eb-9f5c-43e8e53ba1c1.PNG" width ="600">

- 장점 : 생산성이 높다. 역할 분담하여 빠르게 구현. 
- 단점 : 애플 mvc 의 경우 많은 코드들이 Controller 에 집중되어 커지게 된다. 후에 유지보수가 어렵다는 문제 존재.
---
### MVVM(Model-View-ViewModel)

- 출처 : https://medium.com/ios-os-x-development/ios-architecture-patterns-ecba4c38de52
- 출처 : https://eunjin3786.tistory.com/31?category=837198
