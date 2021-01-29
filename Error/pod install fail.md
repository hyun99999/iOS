## 'pod install' fails on Apple Silicone M1 Mac

- M1 Mac 에 대해서
```swift
pod install
```
fail 현상이 발생했다.
<img src ="https://user-images.githubusercontent.com/69136340/106242563-38cd9b00-624b-11eb-968d-8ee0a6f4845f.png" width="800">

## 해결
1. Locate Terminal.app in Finder. (Applications->Terminal.app)
2. Right-click and choose Get Info
3. Check the “Open using Rosetta”
4. Quit all instances of Terminal app and run it again
5. Run sudo gem install ffi
6. Run pod install 

- 출처 : https://armen-mkrtchian.medium.com/run-cocoapods-on-apple-silicon-and-macos-big-sur-developer-transition-kit-b62acffc1387
