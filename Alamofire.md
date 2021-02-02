# CocoaPods

- CocoaPods 는 코코아 프로젝트를 위한 패키지 관리 도구이다. 프로젝트에 필요한 외부 라이브러리를 설치하기 쉽게 도와준다.
- CocoaPods 을 통해서 설치가능. 코코아팟은 ruby 언어를 통해 만들어진 관리 도구이다.

- 터미널에 다음과 같이 입력한다.
```swift
sudo gem install cocoapods
```
gem 은 ruby 패키지 관리 도구이다. cocoapods 이 루비로 쓰여진 프로그램이기에, 루비 패키지 관리 도구를 이용해서 설치한다.

# Foundation Framework
- 파운데이션 프레임워크는 데이터 처리, 네트워크 처리, 파일 처리와 같은 필수 기능을 제공합니다.
- 파운데이션 프레임워크에서 제공하는 클래스는 이름 앞에 NS를 붙입니다. 
  - 예를 들어, NSData, NSArray, NSURL은 파운데이션 프레임워크에서 제공하는 클래스입니다.
- 파운데이션 프레임워크를 사용할 때는 프로그램의 상단에 import 문을 입력합니다.
```swift
import Foundation
```

# Alamofire
- 파운데이션 프레임워크에서는 API호출하기 위해 URLRequest객체를 만들어서 사용했지만, Alamoifire는 더욱 간편하게 접근 할 수 있도록 함.
- Alamofire 는 비동기 기반으로 네트워크 응답을 처리하기 때문에, 응답 메시지를 reponse 메소드의 결과값으로 반환받을 수 없다. 서버에서 응답이 도착했을때 실행될 로직을 클로저 형태로 작성해, reponse 메소드에 넣어주어야 한다(콜백 함수).
- Alamofire 는 서버에서 응답이 도착하면 이를 클로저의 매개변수에 담아 호출한다.

- 예시
```swift
//현재 시간 GETG 호출
func callCurrentTime() { 	
AF.request("API URL. AF.request는 매개변수로 string을 받는다.").responseString() { response in
                switch response.result {
                case .success:
                    self.currentTime = try! response.result.get()
                case .failure(let error):
                    print(error)
                    return
                }
            }
    }

//POST 방식으로 Echo API 호출(httpbody 방식)
func post() {
        // 1. 전송할 값 준비
        let apiURL = "api url string"
        let userId = self.userId
        let userPassword = self.userPassword
        let param: Parameters = [
            "userId": userId,
            "userPassword": userPassword]
        AF.request(apiURL, method: .post, parameters: param, encoding: URLEncoding.httpBody).responseJSON() {
        response in
            switch response.result {
            case .success:
                if let jsonObject = try! response.result.get() as? [String: Any] {
                    let result = jsonObject["result"] as? String
                    let timestamp = jsonObject["timestamp"] as? String
                    let userId = jsonObject["userId"] as? String
                    let userPassword = jsonObject["userPassword"] as? String
                    
                    self.parsedResponse = "요청결과: \(result!)" + "\n"
                        + "응답시간: \(timestamp!)" + "\n"
                        + "요청방식: x-www-form-urlencoded" + "\n"
                        + "유저 ID : \(userId!)" + "\n"
                        + "유저 password: \(userPassword!)" + "\n"
                }
            case .failure(let error):
                print(error)
                return
            }
        }
    }
    
//POST 방식으로 Echo API 호출(JSON 방식)
//request 에서 encodeing:JSONEncoding.default 로 수정.
let headers: HTTPHeaders = [
"Authorization": "some auth",
"Accept": "application/json"
]
AF.request("api url", method: .post, parameters: param, encoding: JSONEnconding.default, headers: headers)
```
- 파라미터 
  - method : 생략할 시 GET방식
  - parameters : 항상 딕셔너리형태
  - encoding :
    - .methodDependent (메소드에 따라 인코딩 타입이 자동으로 결정)
    - .JSONEncoding.default (JSON파일)
    - .queryString (GET 전송에서 사용되는 방식)
    - .httpBody (POST 전송에서 사용되는 방식)
  - headers : 딕셔너리형태

---
>- 출처 :https://ios-development.tistory.com/62?category=894545
>- 출처 :https://velog.io/@budlebee/iOS-Alamofire-를-이용한-API-호출
>- 출처 :
