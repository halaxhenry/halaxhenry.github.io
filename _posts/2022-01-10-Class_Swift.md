\---

layout: post

title: [Swift] 클래스에 대하여!!! 실습

subtitle: Class

categories: "swift"

tags: ["swift"]

\---



# Classes

### Xcode 에서 new --> project --> MacOS --> Command Line Tool

```swift
class MyClass {}
```



### 파일이름과 그 안에 있는 클래스 명을 동일하게 해줍니다!!!



### 우선 하나의 클래스를 만들어 봅시다

```swift
class Enemy {
    var health = 100
    var attackStrength = 10
    
    func move() {
        print("Walk forwards.")
    }
    
    func attack() {
        print("Land a hit, does \(attackStrength)")
    }
}
```



### Enemy 라는 클래스를 만들었습니다. 그 안에는 move 와 attack 이라는 함수를 만들었습니다.



### Initialize를 통해 처음부터 있던 Main.swift 파일 안에서 위의 Enemy 클래스를 불러올수 있습니다.

```swift
let skeleton = Enemy()

print(skeleton.health)

// result : 
// 100
```

### 

### 아래와 같이 함수를 불러올 수 있습니다!!!

```swift
skeleton.move()
skeleton.attack()

// result :
// Walk forwards.
// Land a hit, does 10
```



### 해당 클래스를 불러오기위해 우리는 Initialize 를 해주어야 합니다.



## 클래스는 superClass 를 Inherit (상속) 할 수 있습니다!!!.

### SuperClass 가 뭔가요???

### SuperClass 를 부모 , SubClass를 자식 클래스라고 부릅니다.

### SubClass 는 SuperClass 의 Methods 와 Properties 를 물려받을 수 있습니다.

### 물려 받는 다는 뜻은 즉슨 SuperClass 에 선언되어있는 Methods 나 Properties 들을 SubClass 내에서도 사용 가능하다는 것을 의미합니다.



### 간단한 예를 위해 Enemy 클래스를 Superclass 로 받는 Dragon 이라는 클래스를 만들어보겠습니다

```swift
class Dragon: Enemy {
    
}
```

### 위와 같이 Dragon 클래스는 Enemy를 상속받으며 안에는 아무 것도 없습니다.

### 한번 Main.swift 파일 안에서 Initialize 한뒤 Enemy에서와 같이 move() , attack() 을 call 해보겠습니다.

```swift
let dragon = Dragon()

dragon.move()
dragon.attack()

// result : 
// Walk forwards.
// Land a hit, does 10
```

### 신기하게도 안에 아무것도 없는 Dragon 클래스에 기존과 같이 불렀는데 콘솔창에 프린트가 되는 결과를 볼수 있습니다.



### 이제 Dragon 클래스에 wingSpan 이라는 값을 줘봅시다

```swift
class Dragon: Enemy {
    var wingSpan = 2
}
```



### 상속 받은 클래스를 위와같이 커스터마이징이 가능하며 또한 main.swift 내에서 값에대한 수정도 가능합니다

```swift
let dragon = Dragon()
dragon.wingSpan = 5
```



### wingSpan 은 Dragon 클래스만의 독자적인 값이므로 이전의 skeleton 은 wingSpan 을 사용할수 없습니다



### 하지만 상속 받은 클래스는 상위 클래스의 변수를 사용 및 바꾸는 것이 가능합니다

```swift
let dragon = Dragon()
dragon.wingSpan = 5
dragon.health = 15
```



### Dragon 만의 새로운 func 을 추가 해보겠습니다

```swift
class Dragon: Enemy {
    var wingSpan = 2
    
    func talk(speech: String) {
        print("Says: \(speech)")
    }
}
```



### func 을 만들었으니 한번 사용해봅시다!!!

```swift
dragon.move()
dragon.attack()
dragon.talk(speech: "My teeth are swords! My claws are spears! My wings are a hurricane")

// result :
// Walk forwards.
// Land a hit, does 15
// Says: My teeth are swords! My claws are spears! My wings are a hurricane
```



### 용이니까 걷기보다는 날개 하고 싶네요!!!

```swift
class Dragon: Enemy {
    var wingSpan = 2
    
    func talk(speech: String) {
        print("Says: \(speech)")
    }
    
    override func move() {
        print("Fly forwards")
    }
}
```



### 위와 같이 부모클래스의 func 을 상속 받고 수정하기 위해서는 override 라는 표현이 필요합니다!!!

### 입력했으니 실행해보겠습니다.

```swift
dragon.move()
dragon.attack()
dragon.talk(speech: "My teeth are swords! My claws are spears! My wings are a hurricane")

// result : 
// Fly forwards
// Land a hit, does 15
// Says: My teeth are swords! My claws are spears! My wings are a hurricane
```



### 단순히 드래곤이 공격할때 hit 만 하지 않겠죠!!!! 불도 뿜어야지용용용!!!

### 아래와 같이 super.attack() 를 안에 넣으면 기존의 상속받은 클래스의 func 이 실행됨과 동시에 아래의 새로운 print 도 실행됩니다

```swift
class Dragon: Enemy {
    var wingSpan = 2
    
    func talk(speech: String) {
        print("Says: \(speech)")
    }
    
    override func move() {
        print("Fly forwards")
    }
    
    override func attack() {
        super.attack()
        print("Spits fire, does 10 damages")
    }
}

```



### 한번 main.swift 에서 공격해봅시다!!!

```swift
dragon.move()
dragon.attack()
dragon.talk(speech: "My teeth are swords! My claws are spears! My wings are a hurricane")


// Fly forwards
// Land a hit, does 15      --> 기존의 공격
// Spits fire, does 10 damages     --> 불까지 뿜뿜
// Says: My teeth are swords! My claws are spears! My wings are a hurricane
```

