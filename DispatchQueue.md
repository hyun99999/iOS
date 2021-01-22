# DispatchQueue
- 장점 : 일반 스레드 코드보다 쉽고 효율적으로 코드를 작성할 수 있다. 주로 서버에서 데이터를 내려받거나 이미지, 동영상 등 멀티미디어 처리와 같은 별도의 스레드에서 처리ㄱ 필요하 작업 후 결과를 전달.
- DispatchQueue 를 생성 시 기본은 Serial 이다. Concurrent 유형으로 바꾸려면 별도의 명시 필요.

### 대기열(queue) 유형
  - Serial
    - 대기열에 등록한 순서대로 작업을 실행. 하나의 작업을 실행하고 끝날떄까지 대기열에 있는 다른 작업은 대기.
  - Concurrent
    - 실행중인 작업이 끝나기를 기다리지 않고 대기열에 있는 작업을 동시에 별도의 스레드를 사용하여 실행. 즉, 병렬 처리.

### GCD(Grand Central Dispatch)
- GCD 는 애플에서 만든 멀티 코어환경에서의 application 을 개발할때 도움을주는 라이브러리.Task 라는 작업을 대기열을 주어 순서대로 실행 혹은 동시에 실행시켜주는 등 많은 작업을 한다.
- GCD 를 쓰면 쓰레드를 다루는데 개발자가 직접 관리해주어야할 일이 많이 준다. 모든 Task 를 FIFO 로 관리.

### Main Queue 와 Global Queue
- 엡 실행시 시스템에서 기본적으로 2개의 Queue 만들어준다.
- Main Queue
  > 메인 쓰레드에서 사용되는 Serial Queue.
- Global Queue
  > - 편의상 사용할 수 있게 만든 Concurrent Queue.
  > - Global Queue 는 qos 파라미터를 지원.
  > - 병렬적으로 동시에 처리하기 때문에 완료의 순서는 정할 수 없지만 시작은 할 수 있다.
  ```swift
  // Main Queue
  let mainQueue = DispatchQueue.main
  //Global Queue
  let globalQueue = DispatchQueue.global(qos: .background)
  ```

### QOS. queue 의 우선순위(Quality of Service)
- .userInteractive
  > 제일 높은 우선순위. 유저에게 직접적을 반응을 보여줘야 할 때 사용. UI 업데이트나 애니메이션작업에 사용.
- .userInitiated
  > 유저의 UI 입력을 시작되며, 거의 바로 결과를 보여줘야 할떄 사용. 하지만 작업이 비동기적으로 끝 마칠 수 있다. 로컬에 저장된 문서를 불러올 때와 같이.
- .utility
  > 네트워크나 긴 실행시간을 가졌을때 사용되는 우선순위. 
- .background
  > 직접적을 보여지지 않아도 될때 사용. 속도보다는 에너지 효율에 맞춰 작업 진행.
- .default, .unspecified
  > default 는 userInitiated 와 utility 중간 정도.
  
### Sync / Async
- sync(동기) 와 async(비동기) 라는 메소드를 가지고 있다.
- Serial 과 Concurrent 와는 별개다. 직렬/병렬의 문제.
  
- 출처: https://medium.com/nbt-tech/dispatchqueue는-어떻게-사용할까-44f22f08d62
- 출처: https://magi82.github.io/gcd-01/
