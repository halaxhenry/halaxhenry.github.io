---

layout: post

title: Protocol 에 대한 기초지식

subtitle: Protocol

categories: swift

tags: [swift]

---

# Protocols



Protocol 은 해당 프로토콜을 적용한 class 나 struct 만 프로토콜이 가지고 있는 것들을 사용할 수있도록 제한해주는 좋은 기능입니다.



### 프로토콜을 정의하기

```swift
protocol MyProtocol {
		// define requirement
}
```



### 프로토콜 적용하기

```swift
struct MyStruct: MyProtocol{}
class MyClass: MyProtocol{}
```





예를들어봅시다

Bird 라는 class 를 만들었습니다

```swift
class Bird {
    
    var isFemale = true
    
    func fly() {
        print("I believe I can fly")
    }
    
    func layEgg() {
        if isFemale {
            print("The bird makes a new bird in a shell.")
        }
    }
    
}
```

새의 특징이라고 할수있는 날수있는것 과 알을 낳는 것을 함수로 만들었습니다.

```swift
class Eagle: Bird, CanFly {
    func soar() {
        print("The eagle glides in the air using air currents.")
    }
}

let myEagle = Eagle()

myEagle.fly()
```



독수리는 날수 있으니 별문제가 없겠지만 펭귄의 경우 날수 없습니다.

```swift
class Penguin: Bird {
    func swim() {
        print("The penguin paddles through the water.")
    }
}

let myPenguin = Penguin()
myPenguin.fly()
```



하지만 펭귄도 Bird 클래스를 상속 받아서 날수있게 되는 웃픈....

이러한 상황들을 방지해주기 위해 우리는 protocol을 만들어서 이러한 일을 방지 시켜줍니다

```swift
protocol CanFly {
    func fly()
}

class Bird {
    
    var isFemale = true
    
    func layEgg() {
        if isFemale {
            print("The bird makes a new bird in a shell.")
        }
    }
    
}
```



두둥!!!!

그리고 해당 프로토콜을 적용한 class 나 struct 만이 fly 함수를 사용할 수 있게됩니다.

위와같이 프로토콜 내에 fly 함수를 넣어주므로 Bird 클래스에 있는 fly 함수는 제거 했습니다.

Bird 안에 fly 를 계속 남겨두면 여전히 펭귄도 날수 있는 새로 되겠죠!!!



```swift
class Eagle: Bird, CanFly {
    func fly() {
        print("The eagle flaps its wings and lifts off into the sky.")
    }
    
    func soar() {
        print("The eagle glides in the air using air currents.")
    }
}

let myEagle = Eagle()
myEagle.fly()

// result: 
// The eagle flaps its wings and lifts off into the sky.
```



하지만 프로토콜을 적용하지 않은 펭귄의 경우 아래와 같이 에러를 뿜뿜 하게 됩니다.

```swift
class Penguin: Bird {
    func swim() {
        print("The penguin paddles through the water.")
    }
}

myPenguin.fly()

// result: 
// Error : Value of type 'Penguin' has no member 'fly'
```



여기서 Airplane 이라는 struct 하나를 만들겠습니다.

```swift
struct Airplane: CanFly {
    func fly() {
        print("The airlane uses its engine to lift off into the air")
    }
}

let myPlane = Airplane()
myPlane.fly()
```

비행기도 날수있으니까 위와같이 CanFly 를 적용시키고 !!!

Fly 함수를 사용할 수있죠!!

이렇게 다양한 방법으로 응용할 수 있다는 것이 protocol 의 장점인것같습니다



이전에 봤던 것처럼 class 는 상속이라는 기능이 있지만 struct 는 상속을 할수 없습니다. 

하지만 프로토콜의 경우 struct 도 adopt 할 수 있습니다!!!

```swift
struct MyStruct: CanFly {
    
}

class MyClass: CanFly {
    
}
```





protocol 은 특정 클래스나 스트럭쳐에 한해 해당 메소드를 사용할 수 있도록 도와줍니다.

프토로콜 안에 function 은 body ( {} 안에 내용물 ) 을 가지지 않습니다.

아래와 같이 적어주면 해당 프로토콜을 적용한 class나 struct 만 fly 함수를 사용할수 있겠습니다

```swift
protocol CanFly {
    func fly()
}
```





클래스에 프로토콜을 적용시킬때는 클래스가 상속 받는 superclass 가 있는지 먼저 확인후 

superclass 가 있을 경우 superclass를 가장 앞에 적어주고 그 뒤에 따르는 protocol 들을 적어줘야합니다

```swift
class MyClass: Superclass, FirstProtocolm AnotherProtocols {
    
}
```

