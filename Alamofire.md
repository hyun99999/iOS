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

- 출처 :https://ios-development.tistory.com/62?category=894545
