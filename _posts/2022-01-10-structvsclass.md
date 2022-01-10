# Struct vs. Classes

### Struct 와 Class 를 심도깊게 비교해보자!!!



```swift
// Define the Class
class MyClass: SuperClass {}

// initialise the Class
MyClass()

// Define the Struct
struct MyStruct {}

// initialise the Struct
MyStruct()

```



### 상당히 비슷하죠

### 하지만 Class 는 SuperClass를 상속할 수 있는 반면 Struct 는 상속이 없습니다



### Class 는 수많이 다양한 방법으로 불러질수 있죠

```swift
class Enemy {
		var health: Int
  	var attackStrength: Int
  
  	init()
}

let skeleton = Enemy()
let skeleton1 = Enemy()
```



### 예시 :

```swift
class warrior {
    var health : Int
    var attackStrength: Int
    
    init(health: Int, attackStrength: Int) {
        self.health = health
        self.attackStrength = attackStrength
    }
    
    func printer(){
        print("warrior health = \(health), warrior damage: \(attackStrength)")
    }
}


let romancalvary = warrior(health: 102, attackStrength: 2222)


romancalvary.printer()
```



### 위에 보시면 변수 뒤에 값을 배정하지 않는다면 class의 경우 initialize 해줘야하는 반면에 !!!!!

### Struct 는 initialize 가 필요없습니다.





### 중요한 사실을 하나 짚고 넘어가야할 필요가 있습니다.

### 우선 Enemy 클래스 안에 takeDamage 라는 함수를 만들어볼께요!!!!

```swift
class Enemy {
    var health = 100
    var attackStrength = 10
    
    func takeDamage(amount: Int){
        health = health - amount
    }
    
    func move() {
        print("Walk forwards.")
    }
    
    func attack() {
        print("Land a hit, does \(attackStrength)")
    }
}
```



### 그리고 skeleton1 , skeleton2 를 만들어보겠습니다

### Skeleton2 는 skeleton1을 바탕으로 만들어졌다는걸 아래 코드를 통해 볼수있습니다

### skeleton1에 takeDamage 함수를 준다면 skeleton1의 health는 10 만큼 깎이겠죠!!!

### 그렇다면 skeleton1의 복사본인 skeleton2 에게도 영향을 미칠까요?

```swift
let skeleton1 = Enemy(health: 100, attackStrength: 10)
let skeleton2 = skeleton1

skeleton1.takeDamage(amount: 10)

print(skeleton2.health)

// result :
// 90
```

### 정답은 영향을 미친다 입니다.

### class 는 참조(reference)를 하기에 복사본인 Skeleton2에도 영향을 미치는 겁니다.

### 그러므로 skeleton1 과 skeleton 2를 독립적인 개체로 만들기 위해 skeleton2 = Enemy(health: 100, attackStrength: 10)로 새롭게 만들어줘야합니다



## 대신 위의 Enemy 를 struct 로 만들어준다면

### takeDamage에 mutating 해달라는 에러가 발생할 것입니다

### struct 는 immutable 이라는 것을 알아야되요!!!

```swift
struct Enemy {
    var health = 100
    var attackStrength = 10
    
    mutating func takeDamage(amount: Int){
        health = health - amount
    }
    
    func move() {
        print("Walk forwards.")
    }
    
    func attack() {
        print("Land a hit, does \(attackStrength)")
    }
}
```

![structImmutability.png](https://github.com/halaxhenry/halaxhenry.github.io/blob/main/assets/images/structImmutability.png?raw=true)



```swift
var skeleton1 = Enemy(health: 100, attackStrength: 10)
var skeleton2 = skeleton1

skeleton1.takeDamage(amount: 10)

skeleton1.takeDamage(amount: 10)
skeleton2.takeDamage(amount: 10)

print(skeleton1.health)
print(skeleton2.health)

// result :
// 80
// 90
```

### 이제 제대로 반영이 되었구만유!!! 서로 다른 값으로 분류되었어요!!! 



| Structures                                                   | Classes                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| struct MyStruct {}<br /><br />immutable<br /><br />Passed by Value | class MyClass: SuperClass {}<br /><br />Passed by Reference<br /><br />Inheritance |



### 다 담지 못한 부분이 많은 것 같습니다

### 나머지는 https://docs.swift.org/swift-book/LanguageGuide/ClassesAndStructures.html 을 참조 해야겠습니다

