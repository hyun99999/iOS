# iOS 개발 구조 및 기초 공부
//링크도 걸어두기.

- [MVC](#MVC)
- [기타 파일](#기타-파일)
- [Interface Builder](#Interface-Builder)
- [Segue](#Segue)

## mvc
- model : 데이터 형태를 모아 놓은 것 또는 데이터들의 모임
- View : 사용자 입장에서 보여지는 부분(스토리보드)
- Controller : Model과 View 간의 관계를 연결, 관리
UIViewController 에 View와 Model 을 관리하는 역할이 정의되어 있으므로 모든 ViewController 는 이것을 상속받아서 사용

## 기타 파일
- AppDelegate.swift : 생명 주기 관리를 하는 파일(싱글톤, AppDelegate 구현 한 클래스)
- LaunchScreen.storyboard : 시작화면
- ViewController.swift : view 와 model의 관계를 관리. 보통 한 화면당 하나씩 필요.

## Interface Builder
- 코드를 작성하지 않고 사용자가 인터페이스를 간단하게 작성할 수 있도록 해주는 xcode 안에서의 프로세스
- 코드로 표현x storyboard 상에서 디자인적 표현
1)아울렛 변수(@IBOutlet) : Interface Builder 의 view 를 스위프트 클래스가 참조할 수 있도록 연결된 멤버 변수
2)액션 메소드(@IBAction) : 연결된 멤버 함수

## Segue
- 스토리보드를 통해 출발지와 목적지를 직접 지정하는 방식










출처 : https://ios-development.tistory.com/17?category=889410
