\---

layout: post

title: Swift5 LauchScreen 시간 연장하기

subtitle: LaunchScreen , 시간지연시키기

categories: swift

tags: [swift]

---

# LauchScreen 시간 늘리기!!!

간단간단한거지만 이렇게 요약하면 나중에 누군가에게 도움이 될 수도있고 나에게도 유익할테니까!!! 기록을 남겨보겠습니다.

```swift
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        
        Thread.sleep(forTimeInterval: 1.0)
        
        return true
    }
```



위 method 는 AppDelegate.swift 파일 내에 있어요!!

```swift
Thread.sleep(forTimeInterval: 지연시키고자하는 시간)
```



을 넣어주면 런치 스크린이 원하는 시간만큼 지연되고 사라집니다

아마 이건 잘 응용하면 다른 곳에도 쓸 일이 있지 않을까 싶어용!!!