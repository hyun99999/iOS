# Realm
모바일에 최적화된 데이터베이스 라이브러리.

### Realm
- Realm 은 객체 컨테이너로 동작하기 때문에 `Object` 를 생성해야한다.

### Define Object Model
```swift
// LocalOnlyQsTask is the Task model for this QuickStart
import RealmSwift

class LocalOnlyQsTask: Object {
    @objc dynamic var name: String = ""
    @objc dynamic var owner: String?
    @objc dynamic var status: String = ""
    convenience init(name: String) {
        self.init()
        self.name = name
    }
}
```

### Open a Realm
```swift
// Open the local-only default realm
let localRealm = try! Realm()
```

### Create, Read, Update, and Delete Objects
```swift
// Add some tasks
let task = LocalOnlyQsTask(name: "Do laundry")
try! localRealm.write {
    localRealm.add(task)
}

// Get all tasks in the realm
let tasks = localRealm.objects(LocalOnlyQsTask.self)

// You can also filter a collection
let tasksThatBeginWithA = tasks.filter("name beginsWith 'A'")
print("A list of all tasks that begin with A: \(tasksThatBeginWithA)")

// All modifications to a realm must happen in a write block.
let taskToUpdate = tasks[0]
try! localRealm.write {
    taskToUpdate.status = "InProgress"
}

// All modifications to a realm must happen in a write block.
let taskToDelete = tasks[0]
try! localRealm.write {
    // Delete the LocalOnlyQsTask.
    localRealm.delete(taskToDelete)
}
```

### 출처
출처ㅣhttps://docs.mongodb.com/realm/sdk/ios/install/
