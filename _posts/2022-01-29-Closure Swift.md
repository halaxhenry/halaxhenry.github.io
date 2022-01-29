---



layout: post



title: Swift Closure 복습



subtitle: Closure



categories: swift



tags: [swift]



---

# Closure Swift

익명의 함수 ( 함수명을 가지고 있지 않은 함수) 를 closure 라고 합니다



보통 우리가 알고 있는 함수는 아래와 같은 모형을 가지고 있죠

input 을 넣으면 output 을 내는 구조로 이루어져있습니다 (void 는 output 이 없죠!!!)

```swift
func functionName(parameter: parameterType) -> returnType {
		// Does something
		return output
}
```



함수 선언과 실행은 아래와같이 이루어지는게 우리가 보통 알고 있는 함수입니다.

```swift
func calculator (n1: Int, n2: Int) -> Int {
    return n1 + n2
}

calculator(n1: 2, n2: 3)

// result :
// 5
```



중요한 사실 이있습니다 (뚜둥!!!@)

함수는 함수를 파라미터로 받을 수 있다!!!

빛나는 지식의 별 !!! ✨✨✨✨

아래의 코드와 같이 함수를 받는 함수를 만들 수 도 있어요!!!

```swift
func calculator (n1: Int, n2: Int, operation: (Int, Int) -> Int) -> Int {
    return operation(n1, n2)
}

func add (no1: Int, no2: Int) -> Int {
    return no1 + no2
}

func multiply (no1: Int, no2: Int) -> Int {
    return no1 * no2
}

calculator(n1: 2, n2: 3, operation: add)  // result : 5
calculator(n1: 2, n2: 3, operation: multiply) // result : 6
```



이제 본격적으로 closure 를 만들어볼까요

함수 앞에 붙은 func 함수명을 지우고

아래와 같이 output 데이터타입 뒤에 있던 { 를 맨앞으로 땡겨옵니다

그리고 output 데이터 타입 뒤에 in 을 붙여줍니다 (익숙하지가 않아요....)

```swift
{ (firstNumber: Int, secondNumber: Int) -> Int in
    return firstNumber + secondNumber
}
```



그리고 실행 코자 하는 함수 안에 파라미터로 있는 함수 안에 넣어주면 끝!!!

```swift
calculator(n1: 2, n2: 3, operation: { (no1: Int, no2: Int) -> Int in
    return no1 * no2
})
```

참고로 이미 calculator 함수에서 파라미터로 받을 함수가 어떤 데이터 형을 받을지 정해져있기 때문에 아래와같이 closure 가 받을 파라미터의 데이터형을 생략해줄수 있어요!!!

```swift
calculator(n1: 2, n2: 3, operation: { (no1, no2) -> Int in
    return no1 * no2
})
```



그리고 output 데이터와 return 키워드 또한 생략해줄 수 있답니다

```swift
calculator(n1: 2, n2: 3, operation: { (no1, no2) in no1 * no2 })

calculator(n1: 2, n2: 3, operation: { (no1, no2) in 
    no1 * no2 
})
```



코드가 점점 짧아지는 것을 볼수 있죵!!



또한 우리는 익명의 파라미터를 Closure 에 줄수도 있습니다.



```swift
let result = calculator(n1: 2, n2: 3, operation: {$0 * $1})
print(result) // result : 6
```



점점 짧아지죵!!

```swift
let result = calculator(n1: 2, n2: 3) {$0 * $1}
print(result) // result : 6
```



짧아지는 건 좋지만 코드는 이해할 수 있도록 짜는 것도 중요합니다

그러므로 협업하는 동료와 서로 이해할 수 있도록 짜는 게 중요 하죠

## Have to Think about the balance between **<u>simplicity</u>** and **<u>readability</u>**!!!





아래의 두 코드 들은 같은 결과물 을 출력합니다!!

```swift
let array = [6,2,3,9,4,1]

func addOne(n1: Int) -> Int {
    return n1 + 1
}
array.map(addOne)
```

```swift
let array = [6,2,3,9,4,1]
array.map({$0 + 1})
```



또 한가지의 Tip!! 

아래와 같이 하면 Int 형 데이터를 String 형태로 던져주기도 하죠!!

```swift
let array = [6,2,3,9,4,1]

let newArray = array.map{"\($0)"}
print(newArray)

// result : ["6", "2", "3", "9", "4", "1"]
```



마지막으로

## Closure Expression Syntax

```swift
{(parameters) -> return type in
		statements
}
```

참고하세요!!