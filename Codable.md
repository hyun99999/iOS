# Codable

## Codable
Codable 은 Encodable 과 Decodable 합쳐진 것. 
```swift
public typealias Codable = Decodable & Encodable
```
>- Encodable : data 를 Encoder 에서 변환해주려는 프로터콜로 바꿔주는 것
>- Decodable : data 를 원하는모델로 Decode 해주는 것

**Codable 은 프로토콜**이기 때문에 채택을 하면 된다.

### 출처
출처ㅣhttps://shark-sea.kr/entry/Swift-Codable-알아보기
