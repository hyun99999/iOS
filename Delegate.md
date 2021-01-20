# Delegate
- Delegate (위임하다) : 어떤 객체에서 일어나는 이벤트를 다른 객체에서 처리해주는 것.
> - sender : 일을 시키는 객체
> - receiver : 일을 하는 객체
> - protocol : 해야할 일의 목록
- delegate 는 protocol 로 구현되어있다.

- UITextFieldDelegate 로 textFiled 의 일을 ViewController 에게 위임하는 예제를 살펴보자
```swift
class ViewController: UIViewController, UITextFiledDelegate {

  @IBOutlet var lbText: UILabel!
  @IBOutlet var textField: UITextFiled!
  override func viewDidLoad() {
    super.viewDidLoad()
    //textField 의 일은 ViewController 가 하겠다는 의미. 위임 받겠다.
    textField.delegate = self
  }
  //@IBAction func clickBtn(_ sender: Any) {
  //  버튼 액션 함수 주석처리하고 위임해보자
  //  textField.text = lbtext.text
  //}
  //위임받은 일을 ViewController 에서 구현해보자
  //버튼클릭이 아닌 엔터를 통해서 작동
  func textFieldShouldReturn(_ textField: UITextField) -> Bool {
    lbText.text = textField.text
    return true
  }
}
```
