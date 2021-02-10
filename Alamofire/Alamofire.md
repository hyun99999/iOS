- Alamofire 깃허브 내용을 축약해보자.
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


### 출처
https://github.com/Alamofire/Alamofire/blob/master/Documentation/Usage.md#response-handling
