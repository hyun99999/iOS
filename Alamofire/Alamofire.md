* Alamofire 깃허브 내용을 축약해보자.

# Using Alamofire
## Introduce
alamofire 는 HTTP network requests 의 인터페이스를 제공. Foundation 프레임워크에서 제공하는 Apple 의 URL 로딩 시스템을 기반으로 구축. 즉, URLSession 과 URLSessionTask 하위클래스가 핵심이다. Alamofire 는 이러한 API 와 기타여러 API 를 상요학 쉬운 인터페이스로 래핑해서 제공. 

### The AF Namespace and Reference
이전버전의 Alamofire 문서에서는 `Alamofire.request()` 와 같은 예제를 사용했다. `Alamofire` 접두사가 필요해보였지만 없이도 작동하고 `import Alamofire`로 전역적으로 `request` 메소드와 다른 함수 사용이 가능했다. Alamofire 5 부터는 `AF` 전역이 사용된다. 

## Making Requests
가장 간단하게 `URL`로 전환되느 `String` 을 제공.
```swift
AF.request("https://httpbin.org/get").response { response in
    debugPrint(response)
}
```
> 모든 예제는 `import Alamofire` 가 필요.
이것은 Alamofire의 `Session`타입 의 최상위 API 중 한개다.(보통 `GET` 요청 시 사용) 전체정의는 다음과 같다,
```swift
open func request<Parameters: Encodable>(_ convertible: URLConvertible,
                                         method: HTTPMethod = .get,
                                         parameters: Parameters? = nil,
                                         encoder: ParameterEncoder = URLEncodedFormParameterEncoder.default,
                                         headers: HTTPHeaders? = nil,
                                         interceptor: RequestInterceptor? = nil) -> DataRequest
```
이 메서드는 `DataRequest` 를 생성하는 동시에 메서드와 헤더와 같은 개별적인 components 의 요청 구성을 허용. 또한 요청 별 `RequestInterceptor` 와 `Encodable` 매개변수도 허용.
> `Parameters` 와 `ParameterEncoding` 타입을 활용한 추가 메소드는 더 이상 권장되지 않으며 제거되었다.
API 의 두번째 버전은 더 간단하다.
```swift
open func request(_ urlRequest: URLRequestConvertible, 
                  interceptor: RequestInterceptor? = nil) -> DataRequest
```
이 메서드는 `URLRequestConvertible` 프로토콜을 준수하는 모든 타입의 `DataRequest` 를 생성한다. 

### HTTP Methods
`HTTPMethods` 는 RFC 7231 §4.3 에서 정의된 HTTP methods 이다.
```swift
public struct HTTPMethod: RawRepresentable, Equatable, Hashable {
    public static let connect = HTTPMethod(rawValue: "CONNECT")
    public static let delete = HTTPMethod(rawValue: "DELETE")
    public static let get = HTTPMethod(rawValue: "GET")
    public static let head = HTTPMethod(rawValue: "HEAD")
    public static let options = HTTPMethod(rawValue: "OPTIONS")
    public static let patch = HTTPMethod(rawValue: "PATCH")
    public static let post = HTTPMethod(rawValue: "POST")
    public static let put = HTTPMethod(rawValue: "PUT")
    public static let trace = HTTPMethod(rawValue: "TRACE")

    public let rawValue: String

    public init(rawValue: String) {
        self.rawValue = rawValue
    }
}
```
이런 값들은 `AF.request` 의 `method` argument 로 전달 가능.
```swift
AF.request("https://httpbin.org/get")
AF.request("https://httpbin.org/post", method: .post)
AF.request("https://httpbin.org/put", method: .put)
AF.request("https://httpbin.org/delete", method: .delete)
```
중요한 점은 HTTP methods 가 다름에 따라 다른 의미를 가질 수 있으며 서버가 기대하는 것에 따라 다른 매개변수 인코딩이 필요하는 것.

Alamofire 의 `HTTPMethod` 타입이 지원하지 않는 http methods 를 사용해야하는 경우 사용자 정의 타입도 확장가능하다.
```swift
extension HTTPMethod {
    static let custom = HTTPMethod(rawValue: "CUSTOM")
}
```
### Setting Other `URLRequest` Properties
Alamofire request 생성 메서드는 가장 일반적인 매개변수를 제공하지만 충분하지 않다. `RequestModifer` 클로저를 사용하여 수정가능. 예를들어 `timeoutInterval` 을 5초로 설정하려면 클로저에서 요청을 수정한다.
```swift
AF.request("https://httpbin.org/get", requestModifier: { $0.timeoutInterval = 5 }).response(...)
```
`RequestModifier` 는 trailing closure 구문에서도 작동 가능.
```swift
AF.request("https://httpbin.org/get") { urlRequest in
    urlRequest.timeoutInterval = 5
    urlRequest.allowsConstrainedNetworkAccess = false
}
.response(...)
```
`RequestModifier` 는 URL 과 개별적인 components 를 사용하여 생성된 요청에만 적용되며, `URLRequestConvertible` 값에서 생성된 값에는 모든 매개변수를 직접 설정할 수 있어야 한다. 또한 대부분의 요청을 생성하는 동안 수정해야하기 시작하면 `URLRequestConvertible` 을 채택하는 것이 좋다.

### Request Parameters and Parameter Encoders
Alamofire 는 request 의 매개변수로 모든 `Encodable` 타입 지원한다. 
>- Encodable : 자신을 외부표현(JSON 이라고 생각하면 쉽다.)으로 인코딩 할 수 있는 타입.
이러한 매개변수는 `ParameterEncoder` 프로토콜을 준수하는 유형을 통해서 전달되고 `URLRequest` 에 추가되어 네트워크를 통해 전송. Alamofire 에는 `JSONParameterEncoder` 와 `URLEncoderFormParameterEncoder` 의 두가지가 포함된다.
```swift
struct Login: Encodable {
    let email: String
    let password: String
}

let login = Login(email: "test@test.test", password: "testPassword")

AF.request("https://httpbin.org/post",
           method: .post,
           parameters: login,
           encoder: JSONParameterEncoder.default).response { response in
    debugPrint(response)
}
```

### `URLEncodedFormParameterEncoder`
https://github.com/Alamofire/Alamofire/blob/master/Documentation/Usage.md#introduction 까지 함.

### 출처
https://github.com/Alamofire/Alamofire/blob/master/Documentation/Usage.md#response-handling
