## View Controller Life Cycle(생명주기)

<img src = "https://user-images.githubusercontent.com/69136340/104878832-0648b600-59a0-11eb-9b33-d06d82a542b5.jpg" width="350">

1. loadView()
- 화면에 띄워줄 view 를 만드는 메소드로 **view 를 만들고 메모리에 로드**
- 만약에 뷰와 뷰 컨트롤러를 만들기 위해서 Interface Builder를 사용했다면 이 메서드를 override 하지 말아라.
---
2. viewDidLoad()
- **뷰의 컨트롤러의 뷰가 메모리에 로드 된 후에 호출되며** 시스템에 의해 자동으로 호출이 된다.
- 이 메서드를 재정의하여 사용자에게 화면이 보여지기 전에 데이터를 뿌려주는 리소스를 초기화하는 행위나 초기화면을 구성하는 코드로 작성된다.
- ViewController 생에 딱 한번 호출.
---
3. viewWillAppear
- 뷰 컨트롤러의 화면이 로드된 후 뷰가 화면에 나타나기 직전에 호출.**(뷰 컨트롤러의 뷰가 뷰 계층에 추가)**
- 화면이 나타날 때마다 수행해야하는 작업을 정의하기 좋다. 화면전환을 통해 다시 현재의 화면으로 돌아올 때 viewDidLoad 가 아닌 veiwWillAppear 가 호출된다.(앞으로 나오거나 숨김 해제시에도 호출)
<img src ="https://user-images.githubusercontent.com/69136340/104886941-e61ff380-59ad-11eb-862a-e87c597affa1.png" width="600">
- 이 메서드의 기본구현은 아무 작업도 수행하지 않는다.
---
4.viewDidAppear
- **뷰 컨트롤러의 뷰가 화면으로 완전히 전환 될 때 호출.**
- 이 메서드를 재정의하여 뷰가 나타난 다음에 실행할 애니메이션이나 작업 등을 수행 할 수 있다.
- 이 메서드의 기본구현은 아무 작업도 수행하지 않는다.
---
5. viewWillDisappear
- **뷰컨트롤러의 뷰가 뷰 계층에서 제거되려고 할 때 호출**(화면이 전환하기 전이나 사라지기 직전에 호출)
- 이 메서드의 기본구현은 아무 작업도 수행하지 않는다.
---
6. viewDidDisappear
- **뷰컨트롤러의 뷰가 뷰 계층에서 제거 된 후 호출**(화면이 사라지고 난 후 멈춰야하는 작업들을 할 수 있다. 리소스 해제 등)
- 이 메서드의 기본구현은 아무 작업도 수행하지 않는다.

- 출처 : https://tono18.tistory.com/11?category=837544
- 출처 : https://developer.apple.com
