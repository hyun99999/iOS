# DispatchQueue
- 장점 : 일반 스레드 코드보다 쉽고 효율적으로 코드를 작성할 수 있다. 주로 서버에서 데이터를 내려받거나 이미지, 동영상 등 멀티미디어 처리와 같은 별도의 스레드에서 처리ㄱ 필요하 작업 후 결과를 전달.
- DispatchQueue 를 생성 시 기본은 Serial 이다. Concurrent 유형으로 바꾸려면 별도의 명시 필요.

### 대기열(queue) 유형
  - Serial
    - 대기열에 등록한 순서대로 작업을 실행. 하나의 작업을 실행하고 끝날떄까지 대기열에 있는 다른 작업은 대기.
  - Concurrent
    - 실행중인 작업이 끝나기를 기다리지 않고 대기열에 있는 작업을 동시에 별도의 스레드를 사용하여 실행. 즉, 병렬 처리.
    
### queue 의 우선순위 정하기
- .userInteractive
  - 제일 높은 우선순위. 유저에게 직접적을 반응을 보여줘야 할 때 사용. UI 업데이트나 애니메이션작업에 사용.
- .userInitiated
  - 유저의 UI 입력을 시작되며, 거의 바로 결과를 보여줘야 할떄 사용. 하지만 작업이 비동기적으로 끝 마칠 수 있다. 로컬에 저장된 문서를 불러올 때와 같이.
- .utility
  - 네트워크나 긴 실행시간을 가졌을때 사용되는 우선순위. 
- .background
  - 직접적을 보여지지 않아도 될때 사용. 속도보다는 에너지 효율에 맞춰 작업 진행.
- .default, .unspecified
  - default 는 userInitiated 와 utility 중간 정도.
  
  
  
- 출처: https://medium.com/nbt-tech/dispatchqueue는-어떻게-사용할까-44f22f08d62
