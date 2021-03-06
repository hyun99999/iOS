## TableViewDelegate & TableViewDataSource
- TableViewDelegate
  - 테이블뷰 델리게이트 객체는 UITableViewDelegate 프로토콜을 채택한다.
  - MVC 패턴 중 Controller 와 관련이 있다.
  - 델리게이트는 테이블뷰의 **시각적인 부분 수정, 행의 선택 관리, 액세서리뷰 지원, 테이블뷰의 개별 행 편집** 가능.
```swift
// 지정된 행이 선택되었음을 알리는 메서드
func tableView(UITableView, didSelectRowAt: IndexPath)

// 지정된 행의 선택이 해제되었음을 알리는 메서드
func tableView(UITableView, didDeselectRowAt: IndexPath)

// 특정 위치 행의 높이를 묻는 메서드
func tableView(UITableView, heightForRowAt: IndexPath)

// 특정 위치 행의 들여쓰기 수준을 묻는 메서드
func tableView(UITableView, indentationLevelForRowAt: IndexPath)

// 특정 섹션의 헤더뷰 또는 푸터뷰를 요청하는 메서드
func tableView(UITableView, viewForHeaderInSection: Int)
func tableView(UITableView, viewForFooterInSection: Int)

// 특정 섹션의 헤더뷰 또는 푸터뷰의 높이를 물어보는 메서드
func tableView(UITableView, heightForHeaderInSection: Int)
func tableView(UITableView, heightForFooterInSection: Int)

// 테이블뷰가 편집모드에 들어갔음을 알리는 메서드
func tableView(UITableView, willBeginEditingRowAt: IndexPath)

// 테이블뷰가 편집모드에서 빠져나왔음을 알리는 메서드
func tableView(UITableView, didEndEditingRowAt: IndexPath?)
```
- TableViewDataSource
  - 테이블뷰 데이터 소스 객체는 UITableViewDataSource 프로토콜을 채택합니다.
  - 테이블뷰를 생성하고 수정하는데 필요한 정보를 테이블뷰 객체에 제공한다.
  - MVC 패턴 중 Model 과 관련이 있다.
  - UITableView 객체에 섹션의 수와 행의 수를 알려주며 행의 삽입, 삭제 및 재정렬하는 기능을 선택적으로 구현 가능하다.
  - @required 로 선언된 두 가지 메서드는 UITableViewDataSource 프로토콜을 채택하 타입에 필수로 구현해야 한다.
```swift
@required 
// 특정 위치에 표시할 셀을 요청하는 메서드
func tableView(UITableView, cellForRowAt: IndexPath) 
 
// 각 섹션에 표시할 행의 개수를 묻는 메서드
func tableView(UITableView, numberOfRowsInSection: Int)
 
@optional
// 테이블뷰의 총 섹션 개수를 묻는 메서드
func numberOfSections(in: UITableView)
 
// 특정 섹션의 헤더 혹은 푸터 타이틀을 묻는 메서드
func tableView(UITableView, titleForHeaderInSection: Int)
func tableView(UITableView, titleForFooterInSection: Int)
 
// 특정 위치의 행을 삭제 또는 추가 요청하는 메서드
func tableView(UITableView, commit: UITableViewCellEditingStyle, forRowAt: IndexPath)
 
// 특정 위치의 행이 편집 가능한지 묻는 메서드
func tableView(UITableView, canEditRowAt: IndexPath)

// 특정 위치의 행을 재정렬 할 수 있는지 묻는 메서드
func tableView(UITableView, canMoveRowAt: IndexPath)
 
// 특정 위치의 행을 다른 위치로 옮기는 메서드
func tableView(UITableView, moveRowAt: IndexPath, to: IndexPath)
```
## UITableView 설정
- TableViewController 클래스에 UITableViewDelegate, UITableViewDataSource 프로토콜을 채택한다.
- storyboard 의 테이블뷰와 연결.
- viewDidLoad() 에서 delegate 와 datasource 를 `self` 로 연결

## dequeReusableCell 
- 재사용 가능한 셀을 큐에서 빼와서 특정 identifier 의 셀을 가져오는 메소드.
```swift
func dequeueResuableCell(withIdentifier identifier: String) -> UITableViewCell?
```
- iOS 기기는 한정된 메모리를 가지 애플리케이션을 구동한다. 반복된 뷰를 사용하기보다는 뷰를 재사용 할 수 있다.

> 1. 셀을 표시하기 위해서 데이터소스에 인스턴스를 요청. func tableView(UITableView, cellForRowAt: IndexPath) 
> 2. 데이터소스는 요청마다 새로운 셀을 만드는 대신 재사용 큐(Reuse Queue)에 재사용을 위해 대기하고 있는 셀이 있는지 확인 후 있다면 새로운 데이터를 설정하고 없으면 셀을 생성.
> 3. 데이터소스가 셀을 반환하면 테이블뷰 및 컬렉션뷰는 셀을 화면에 표시한다.
> 4. 사용자가 스크롤을 하게 되면 이부 셀들이 화면 밖으로 사라지면서 다시 재사용 큐에 들어간다.
> 5. 위의 과정 반복.

- `dequeReuseableCell` 메소드의 파라미터로 들어가는 identifier 는 storyboard 의 UITableViewCell 에 등록한 identifier 이다.

## tableViewCell 종류
- Basic, Right Detail, Left Detail, Subtitle, Custom
<img src ="https://user-images.githubusercontent.com/69136340/109510768-beac6280-7ae5-11eb-9f62-dd40ee2591c4.png" width = "600">

### 출처 
출처ㅣhttps://woonhyeong.tistory.com/6?category=827228
