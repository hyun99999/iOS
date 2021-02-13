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
`URLEncodedFormParameterEncoder` 는 기존 URL query stirng 또는 요청의 HTTP body 로 설정될 URL 인코딩 문자열로 값을 인코딩. destination of the encoding 을 설정하여 인코딩 된 문자열이 설정된 위치를 제어 가능. `URLEncodedFormParameterEncoder.Destination` 에는 세가지 경우가 있다.
  * `.methodDependent` - 인코딩 된 query 문자열 결과를 .get, .head, .delete 요청에 대한 기존 query 문자열에 적용하고 다른 HTTP 메소드의 요청에 대한 HTTP body 로 설정.
  * `.queryString` - request `URL` query 에 인코딩 된 문자열을 설정하거나 추가.
  * `.httpBody` - 인코딩된 문자열을 `URLRequest` 의 HTTP body 로 설정.
  
HTTP body 와 함께 encoded 된 request 의 `Content-Type` HTTP header 는 기본값이 `application/x-www-form-urlencoded; charset=utf-8` 로 설정된다.

**GET Request With URL-Encoded Parameters**
```swift
let parameters = ["foo": "bar"]

// All three of these calls are equivalent
AF.request("https://httpbin.org/get", parameters: parameters) // encoding defaults to `URLEncoding.default`
AF.request("https://httpbin.org/get", parameters: parameters, encoder: URLEncodedFormParameterEncoder.default)
AF.request("https://httpbin.org/get", parameters: parameters, encoder: URLEncodedFormParameterEncoder(destination: .methodDependent))

// https://httpbin.org/get?foo=bar
```
**POST Request With URL-Encoded Parameters**
```swift
let parameters: [String: [String]] = [
    "foo": ["bar"],
    "baz": ["a", "b"],
    "qux": ["x", "y", "z"]
]

// All three of these calls are equivalent
AF.request("https://httpbin.org/post", method: .post, parameters: parameters)
AF.request("https://httpbin.org/post", method: .post, parameters: parameters, encoder: URLEncodedFormParameterEncoder.default)
AF.request("https://httpbin.org/post", method: .post, parameters: parameters, encoder: URLEncodedFormParameterEncoder(destination: .httpBody))

// HTTP body: "qux[]=x&qux[]=y&qux[]=z&baz[]=a&baz[]=b&foo[]=bar"
```
### Configuring the Sorting of Encoded Values
Swift 4.2 이후로 딕셔너리 타입에서 사용하는 해싱 알고리즘은 런타임에 앱 실행마다 다른 임의의 내부 순서를 생성. 이로인해 인코딩된 매개변수의 순서 달라져 기타동작에 영향을 미칠 수 있다.`URLEncodedFormEncoder` 는 기본적으로 인코딩된 key-value 쌍을 정렬한다. `Encodable` 타입에 대해서 상수출력을 제공하지만 실제 인코딩 순서와 일치하지 않을 수 있다. `alphabetizeKeyValuePairs` 를 `false` 로 설정하여 구현순서로 돌아갈 수 있지만, 이 경우에도 `Dictionary` 순서가 무작위로 지정된다. 

사용자의 `URLEncodedFormParameterEncorder` 를 만들고 `URLEncodedFormEncoder` 의 이니셜라이저에서 요구하는 `alphabetizeKeyValuePairs` 를 지정할 수 있다.
```swift
let encoder = URLEncodedFormParameterEncoder(encoder: URLEncodedFormEncoder(alphabetizeKeyValuePairs: false))
```

### Configuring the Encoding of `Array` Parameters
collection 타입을 인코딩하는 방법에 대해 제시된 특별한 사양이 없기 때문에 기본적으로 Alamofire 는 key 에 [] 를 추가한다. `foo [] = & foo [] = 2` 형식으로 인코딩합니다. 

`URLEncodedFormEncoder.ArrayEncoding` 열거형은 `Array` 매개변수의 인코딩을 위해서 다음의 메서드를 제공합니다.
* `.brackets` - 모든 값에 대해 key에 [] 가 추가된다. 이것이 기본이다.
* `.noBrackets` - [] 가 추가되지 않는다. 키는 있는 그대로 인코딩된다.

기본적으로 Alamofire 는 `.brackets` 인코딩을 사용한다. `foo = [1,2]` 는 `foo[]=1&foo[]=2` 으로 인코딩된다.

`.noBrackets` 을 사용하면 `foo = [1,2]` 는 `foo=1&foo=2` 으로 인코딩된다.

고유한 `URLEncodedFormParameterEncoder` 를 만들고 전달된 `URLEncodedFormEncoder` 의 이니셜라이저에서 요구하는 `ArrayEncoding` 을 지정할 수 있다.

```swift
let parameters: [String: [String]] = [
    "foo": ["1", "2"]
]
let encoder = URLEncodedFormParameterEncoder(encoder: URLEncodedFormEncoder(arrayEncoding: .noBrackets))

AF.request("https://httpbin.org/post", method: .post, prameters, encoder: encoder)

//출처ㅣ https://velog.io/@wimes/Alamofire-%EB%B6%84%EC%84%9D
```

### Configuring the Encoding of Bool Parameters
`URLEncodedFormEncoder.BoolEncoding` 열거형은 `Bool` 매개변수를 인코딩하기위해서 다음 메서드를 제공합니다.

* `.numeric` - `true` 를 1 로 `false` 를 0 으로 인코딩. 기본케이스이다.
* `.literal` - `true`, `false` 를 문자열 리터럴로 인코딩.

고유한 `URLEncodedFormParameterEncoder` 를 만들고 전달된 `URLEncodedFormEncoder` 의 이니셜라이저에서 요구하는 `BoolEncoding` 을 지정할 수 있다.

```swift
let encoder = URLEncodedFormParameterEncoder(encoder: URLEncodedFormEncoder(boolEncoding: .numeric))
```

### Configuring the Encoding of `Data` Parameters
`DataEncoding` 은 `Data` 인코딩을 위해서 다음의 메서드를 포함합니다.

* `.deffedToData` - 데이터의 기본 `Encodable` 지원을 사용.
* `.base64` - `Data` 를 Base64 로 인코딩된 문자열로 인코딩. 기본 케이스이다.
* `.custom((Data) -> throws -> String)` - 주어진 클로저를 사용하여 `Data` 를 인코딩.

고유한 `URLEncodedFormParameterEncoder` 를 만들고 전달된 `URLEncodedFormEncoder` 의 이니셜라이저에서 요구하는 `DataEncoding` 을 지정할 수 있다.

```swift
let encoder = URLEncodedFormParameterEncoder(encoder: URLEncodedFormEncoder(dataEncoding: .base64))
```

### Configuring the Encoding of `Date` Parameters
`Date` 를 `String` 으로 인코딩하려는 수많은 방법을 고려할 때 `DateEncoding` 에는 `Date` 매개변수를 인코딩하는 다음 메서드를 포함한다.

* `.deferredToDate` - `Date` 의 `Encodable` 지원을 사용. 기본 케이스이다.
* `.secondsSince1970` - 1970년 1월 1일 자정 UTC 이후 날짜를 초로 인코딩.
* `.millisecondsSince1970` - 1970년 1월 1일 자정 UTC 이후 날짜를 milliseconds 로 인코딩.
* `.iso8601` - ISO 8601 과 RFC3339 표준에 따라 날짜를 인코딩.
* `.formatted(DateFormatter)` - 주어진 `DataFormatter` 를 사용해서 날짜를 인코딩.
* `.custom((Date) throws -> String)` - 주어진 클로저를 사용햇 날짜를 인코딩.

고유한 `URLEncodedFormParameterEncoder` 를 만들고 전달된 `URLEncodedFormEncoder` 의 이니셜라이저에서 요구하는 `DateEncoding` 을 지정할 수 있다.

```swift
let encoder = URLEncodedFormParameterEncoder(encoder: URLEncodedFormEncoder(dateEncoding: .iso8601))
```
### Configuring the Encoding of Coding Keys
다양한 매개변수 key 스타일 때문에 `KeyEncoding` 은 `lowerCamelCase` 의 키에서 키 인코딩을 커스터마이즈 할 수 있는 메서드를 제공한다. 

* `.useDefaultKeys` - 각 타입에 지정된 키를 사용. 기본 케이스이다.
* `.convertToSnakeCase` - `oneTwoThree` becomes `one_two_three`.
* `.conVertToKebabCase` - `oneTwoThree` becomes `one-two-three`.
* `.capitalized` - a.k.a `UpperCamelCase`: `oneTwoThree` becomes `OneTwoThree`.
* `.uppercased` - `oneTwoThree` becomes `ONETWOTHREE`
* `.lowecased` - `oneTwoThree` becomes `onetwothree`.
* `.custom((String) -> String)` - 주어진 클로저를 사용해서 인코딩.

고유한 `URLEncodedFormParameterEncoder` 를 만들고 전달된 `URLEncodedFormEncoder` 의 이니셜라이저에서 요구하는 `KeyEncoding` 을 지정할 수 있다.

```swift
let encoder = URLEncodedFormParameterEncoder(encoder: URLEncodedFormEncoder(keyEncoding: .convertToSnakeCase))
```

### Configuring the Encoding of Spaces
이전 형식의 인코더는 `+` 를 사용해서 공백을 인코딩하고 일부 서버는 여전히 최신 백분율 인코딩 대신에 이 인코딩을 기대하므로 alamofire 는 공백코딩을 위해서 다음 메서드를 포함한다.

* `.percentEscaped` - 표준 퍼센트 이스케이핑을 적용해서 공백문자를 인코딩한다. `" "` 는 `%20` 으로 인코딩. 이것이 기본 케이스이다.
* `.plusReplaced` - `" "` 를 `+` 로 대체해서 인코딩.

고유한 `URLEncodedFormParameterEncoder` 를 만들고 전달된 `URLEncodedFormEncoder` 의 이니셜라이저에서 요구하는 `SpaceEncoding` 을 지정할 수 있다.

```swift
let encoder = URLEncodedFormParameterEncoder(encoder: URLEncodedFormEncoder(spaceEncoding: .plusReplaced))
```

### `JSONParameterEncoder`

https://github.com/Alamofire/Alamofire/blob/master/Documentation/Usage.md#jsonparameterencoder 여기서부터.

https://velog.io/@wimes/Alamofire-%EB%B6%84%EC%84%9D 참고

### 출처
https://github.com/Alamofire/Alamofire/blob/master/Documentation/Usage.md#response-handling
