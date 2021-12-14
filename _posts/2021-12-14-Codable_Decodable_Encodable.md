---
layout: post
title: Data 관리 - Codable, Decodable, Encodable
subtitle: Codable, Decodable, Encodable
categories: swift
tags: [swift]
---

# Codable 이란?

## A type that can convert itself into and out of an external representation.

### 간단히 말하여 자신을 변환하거나 외부표현 (Json 양식 등) 으로 변환할 수 있는 타입을 말합니다



## Codable = Decodable + Encodable



### Codable 은 간단히 Encodable 과 Decodable 을 통틀어 말합니다



```swift
typealias Codable = Decodable & Encodable
```





## Codable , Decodable, Encodable 은 간단히 말해 Protocol 입니다.

### Protocol 이라는 것은 즉슨 enum, class, struct 에서 선택하여 사용할 수 있음을 의미하겠죠!!!!

### codable 을 사용하게 되면 데이터를 json 과 같은 외부 표현으로 만들수 있습니다

# Encodable

## A type that can encode itself to an external representation.

### 말 그대로 자기자신 (데이터)를 외부표현 (Json) 과 같은 형태로 바꿔주는 타입을 말합니다

```swift
protocol Encodable
```

# Decodable

## A type that can decode itself from an external representation

### Encodable 과 반대로 이것은 외부 자료를 해석 (디코딩) 해주는 것을 말합니다.

```swift
protocol Decodable
```

## 중요한 사실은, <b><u>Decodable</u> </b>을 채택하는 데이터는 <b><u>init함수</u> </b>를 필수적으로 가져야합니다



## 

# Codable 을 사용해보자!!!!



```swift
import UIKit


struct Track {
  let title: String
  let artistName: String
  let isStreamable: Bool
}
```



## 여기에 Codable 이라는 Protocol 을 채택해줍니다

```swift
struct Track: Codable {
  let title: String
  let artistName: String
  let isStreamable: Bool
}
```



## Encoding 하기

### 이제 위의 struct의 인스턴스를 <u>JSONEncoder</u>를 이용하여 Data로 Encoding 해보겠습니다.

```swift
let sampleInput = Track(title: "New Rules", artistName: "Dua Lipa", isStreamable: true)

do {
    let encoder = JSONEncoder()
  
    let data = try encoder.encode(sampleInput)
  
    print(data) 
  
// 출력 결과 :
// 		65 Bytes
  
} catch {
    print(error)
}
```



### 위의 data는 변환된 JSON data 이므로 byte 값이 출력이 되요!!!. 이것을 String 형태로 바꿔보도록 하겠습니다.



```swift
do {
    let encoder = JSONEncoder()
    
  	let data = try encoder.encode(sampleInput)
    
  	if let jsonString = String(data: data, encoding: .utf8) {
      print(jsonString) 
      
 // 출력 결과 :
 // 		{"title":"New Rules","isStreamable":true,"artistName":"Dua Lipa"}
      
    }
} catch {
    print(error)
}
```



### 기본적으로 JSON Encoder 는 객체를 단일행 JSON 구조로 인코딩하는데 OutputFormatting 설정을 추가하여 출력에 줄 바꿈과 탬을 삽입하거나, key를 정렬하여 가독성을 높이도록 JSON Encoder를 구성할 수 있습니다



```swift
do {
    let encoder = JSONEncoder()
    
    // 줄바꿈과 들여쓰기 삽입
    encoder.outputFormatting = .prettyPrinted
    
    // 키 정렬 (사전순)
    encoder.outputFormatting = .sortedKeys
    
    // 두 가지 설정을 함께 쓰는 경우 
    encoder.outputFormatting = [.sortedKeys, .prettyPrinted]
    
    let data = try encoder.encode(sampleInput)
    
    if let jsonString = String(data: data, encoding: .utf8) {
        print(jsonString) 
        
        // 출력 결과 :
        //{
        //  "artistName" : "Dua Lipa",
        //  "isStreamable" : true,
        //  "title" : "New Rules"
        //}
        
    }
} catch {
    print(error)
}
```



<hr>

# Decoding 하기



### Decoding 역시 JSONDecoder 를 이용하여 간단히 구현이 가능합니다.

```swift
let jsonData = """
{
  "artistName" : "Dua Lipa",
  "isStreamable" : true,
  "title" : "New Rules"
}
""".data(using: .utf8)!


do {
    let decoder = JSONDecoder()
    let data = try decoder.decode(Track.self, from: jsonData)
    print(data) 
    // 출력 결과 :
    //     Track(title: "New Rules", artistName: "Dua Lipa", isStreamable: true)
    print(data.title) // New Rules
} catch {
    print(error)
}
```



### 정말 간단합니다!!! 그럼 실제로 적용하면서 마주할 수 있는 다양한 경우와 해결 방법에 대해 알아보겠습니다.



<hr>



## Nullable & Optional Key

### 특정 필드의 값이 nullable 하거나, optional 한 키의 경우 아래와 같이 struct 를 정의 할 수 있습니다.

```swift
struct Track: Codable {
    let title: String
    let artistName: String
    let isStreamable: Bool?
}
```



### 만약 Default 값을 특정한 값에 부여하고 싶다면!!!

```swift
struct Track: Codable {
    let title: String
    let artistName: String
    let isStreamable: Bool = true
}
```



<hr>



## Customizing Key Names (key 이름 변경하기)

### api 응답으로 받은 데이터의 경우 스네이크 케이스 (snake_case)를 사용하는 경우가 있는데 Swift 이름 정의규칙에 부합하지 않으므로 이에 맞게 수정해주고 싶을때 CodingKeys 를 사용합니다.

# CodingKey



## A type that can be used as a key for encoding and decoding

```swift
protocol CodingKey
```

### 위에서 볼 수 있듯이 CodingKey 또한 Protocol 이네요!!!!

### CodingKey는 Encoding/Decoding 을 위한 키로 사용할수 있는 타입입니다!!!



### JSON 데이터의 key와, 사용하고자 하는 key가 맵핑 될 수 있도록 CodingKey를 채택한 enum을 먼저 만들어주겠습니다.

```swift
struct Track: Codable {
    let title: String
    let artistName: String
    let isStreamable: Bool
    
    enum CodingKeys: String, CodingKey {
        case title = "track_name"
        case artistName = "artist_name"
        case isStreamable = "is_streamable"
    }
}
```



<hr>

## Handling Dates

### JSON 에는 날짜를 나타내는 데이터 유형이 없으므로 클라이언트와 서버가 동의한 형식 (일반적으로 <u>ISO 8601</u>)으로 Serialize 하게 되요!!!



```swift
struct Track: Codable {
    let title: String
    let artistName: String
    let isStreamable: Bool
    let releaseDate: Date
}

let jsonData = """
{
  "artistName" : "Dua Lipa",
  "isStreamable" : true,
  "title" : "New Rules",
  "releaseDate": "2017-06-02T12:00:00Z"
}
""".data(using: .utf8)!

do {
    let decoder = JSONDecoder()
    let data = try decoder.decode(Track.self, from: jsonData)
    print(data)
    print(data.releaseDate)
} catch {
    print(error)
}


// 출력 결과 :
// typeMismatch(Swift.Double, Swift.DecodingError.Context(codingPath: [CodingKeys(stringValue: "releaseDate", intValue: nil)], debugDescription: "Expected to decode Double but found a string/data instead.", underlyingError: nil))

```

### 보다시피 앞에서 정의한 Track 구조에 Date 타입의 releaseDate 를 추가로 정의하고 Decode 해보면 "typeMismatch" 에러가 발생합니다



### 이 경우 아래와 같이 decoder 에 dateDecodingStrategy를 정의 해주면 serialize 날짜가 date 형태로 잘 변환되어 있음을 볼 수 있습니다.

```swift
decoder.dateDecodingStrategy = .iso8601
```



```swift
struct Track: Codable {
    let title: String
    let artistName: String
    let isStreamable: Bool
    let releaseDate: Date
}

let jsonData = """
{
  "artistName" : "Dua Lipa",
  "isStreamable" : true,
  "title" : "New Rules",
  "releaseDate": "2017-06-02T12:00:00Z"
}
""".data(using: .utf8)!

do {
    let decoder = JSONDecoder()
    decoder.dateDecodingStrategy = .iso8601   // <--- 요기
    let data = try decoder.decode(Track.self, from: jsonData)
    
    print(data)
    print(data.releaseDate)
} catch {
    print(error)
}

// 출력 결과 :
//		Track(title: "New Rules", artistName: "Dua Lipa", isStreamable: true, releaseDate: 2017-06-02 12:00:00 +0000)
//2017-06-02 12:00:00 +0000

```



<hr>

## Wrapper Keys

### api 응답이 아래와 같이 Wrapper Key를 포함하는 경우,

```json
{
  "resultCount": Int,
  "results" : [Track] // array chunk가 그대로 반환되었네요 이경우 [데이터값] 을 표현해주어야해요
}
```



### 응답에 대한 새로운 타입을 만들어 줍니다.

```swift
struct Response: Codable {
    let resultCount: Int
    let results: [Track]
}

struct Track: Codable {
    let title: String
    let artistName: String
    let isStreamable: Bool
}

let jsonData = """
{
  "resultCount": 50,
  "results": [{
    "artistName" : "Dua Lipa",
    "isStreamable" : true,
    "title" : "New Rules"
  }]
}
""".data(using: .utf8)!

do {
    let decoder = JSONDecoder()
    let data = try decoder.decode(Response.self, from: jsonData)
    print(data.results[0]) 
  // 출력 결과 :
  // 		Track(title: "New Rules", artistName: "Dua Lipa", isStreamable: true)
} catch {
    print(error)
}
```



<hr>

## Root Level Array

### 만약 api 응답이 Root Element 로 배열을 가진다면 아래와 같이 Decoding이 가능합니다

```swift
struct Track: Codable {
    let title: String
    let artistName: String
    let isStreamable: Bool
}

let jsonData = """
[{
    "artistName" : "Dua Lipa",
    "isStreamable" : true,
    "title" : "New Rules"
}]
""".data(using: .utf8)!

do {
    let decoder = JSONDecoder()
    let data = try decoder.decode([Track].self, from: jsonData)
    print(data[0]) 
 	// 출력 결과 : 
  // 		Track(title: "New Rules", artistName: "Dua Lipa", isStreamable: true)
} catch {
    print(error)
}
```



<hr>



## Decode type as Enum

```swift
struct Track: Codable {
    let title: String
    let artistName: String
    let isStreamable: Bool
    let primaryGenreName: Genre
}

enum Genre: String, Codable {
    case Pop
    case KPop = "K-Pop"
    case Rock
    case Classical
    case HipHop = "Hip-Hop"
}

let jsonData = """
{
  "artistName" : "Dua Lipa",
  "isStreamable" : true,
  "title" : "New Rules",
  "primaryGenreName": "Pop"
}
""".data(using: .utf8)!

do {
    let decoder = JSONDecoder()
    let data = try decoder.decode(Track.self, from: jsonData)
    print(data)
    print(data.primaryGenreName) // Pop
} catch {
    print(error)
}
```

## 위의 예제처럼 Track 구조의 primaryGenreName 이 Genre 열거형에서 하나의 값을 가지는 경우 Enum에도 Codable 을 적용하면 그대로 타입에 적용할 수 있습니다.



<hr>

## 만약, enum 에서 정의하지 않은 케이스가 있는 경우, 아래와 같이 Unknown 을 추가로 선언하고, init(from:) 을 구현하여 일치하는 값이 없는 경우 Unknown 으로 매핑시켜주어 오류를 방지할 수 있습니다.



```swift
enum Genre: String, Codable {
    case Pop
    case KPop = "K-Pop"
    case Rock
    case Classical
    case HipHop = "Hip-Hop"
    case Unknown
    
    init(from decoder: Decoder) throws {
      self = try Genre(rawValue: decoder.singleValueContainer().decode(RawValue.self)) ?? .Unknown
   }
}
```



<hr>

## Decoding nested JSON data into a single struct

## 종종 api 응답이 깊이 중첩된 구조로 클라이언트에서 사용하기 불편한 상황이 있을 수 있는데요, 이렇게 api 응답 데이터의 구조를 바꾸고 싶은 경우, Decodable / Encodable 프로토콜을 커스텀 할 수 있습니다.



### 아래와 같은 JSON 데이터를

```json
{
  "artistName": "Dua Lipa",
  "isStreamable": true,
  "title": "New Rules",
  "collection": { 
    "name": "Dua Lipa (Deluxe)", 
    "price": 11.99
  }
}
```



### 아래와 같은 구조체로 디코딩 하는 방법을 알아보겠습니다.

```swift
struct Track {
  "artistName": String,
  "title": String,
  "isStreamable": Bool,
  "collectionName": String,
  "collectionPrice": Double
}
```



### 먼저 Track 구조체로 병합하려는 JSON 개체의 열거형을 정의합니다. 중첩된 키가 여러개라면 열거형도 그 만큼 추가로 정의해 주면 됩니다.



```swift
struct Track {
    let title: String
    let artistName: String
    let isStreamable: Bool
    let collectionName: String
    let collectionPrice: Double
    
    enum CodingKeys: String, CodingKey {
        case title, artistName, isStreamable
        case collectionInfo = "collection"
    }
    
    enum CollectionKeys: String, CodingKey {
        case name
        case price
    }
}
```



### 이제 Decodable 프로토콜을 `init(from:`을 사용하여 커스텀 해 줍니다.

```swift
extension Track: Decodable {
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        self.title = try container.decode(String.self, forKey: .title)
        self.artistName = try container.decode(String.self, forKey: .artistName)
        self.isStreamable = try container.decode(Bool.self, forKey: .isStreamable)
        let collectionInfo = try container.nestedContainer(keyedBy: CollectionKeys.self, forKey: .collectionInfo)
        self.collectionName = try collectionInfo.decode(String.self, forKey: .name)
        self.collectionPrice = try collectionInfo.decode(Double.self, forKey: .price)
    }
}
```



```swift
let values = try decoder.container(keyedBy: CodingKeys.self)
```



### 위의 코드는 `CodingKeys` 열거형의 키를 사용하는 컨테이너를 추출하며, 여기에서 추출된 컨테이너에서 `nestedContainer`를 사용하여 아래와 같이 중첩된 컨테이너를 추출할 수 있습니다.



```swift
let collectionInfo = try decoder.nestedContainer(keyedBy: CollectionKeys.self, forKey: .collectionInfo)
```



## Encoding a struct into nested JSON data

### 반대로 구조체를 중첩된 JSON 데이터로 변환해 보겠습니다.

### 디코딩 할 때와 반대로 컨테이너에 값을 인코딩 해주면 됩니다. 이 때 컨테이너는 변경할 수 있는 mutable 프로퍼티 이므로 var로 선언해 줍니다.

```swift
extension Track: Encodable {
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        var collectionInfo = container.nestedContainer(keyedBy: CollectionKeys.self,
                                                       forKey: .collectionInfo)
        try container.encode(title, forKey: .title)
        try container.encode(artistName, forKey: .artistName)
        try container.encode(isStreamable, forKey: .isStreamable)
        try container.encode(title, forKey: .title)
        try collectionInfo.encode(collectionName, forKey: .name)
        try collectionInfo.encode(collectionPrice, forKey: .price)
    }
}
```





# 마무리

여기까지 Swift의 Codable 프로토콜의 기본 사용법과 활용법에 대해 알아보았습니다. Codable이 나오기 전까진 JSON을 다루기 위해 라이브러리를 사용하는 등 어려운 점이 많았는데 이렇게 간단하게 인코딩/디코딩을 할 수 있다니 정말 유용한 기능인 것 같습니다.

감사합니다.

# 좋은 참고 자료  :

## https://zeddios.tistory.com/search/decodable

## https://jouureee.tistory.com/93

## https://medium.com/humanscape-tech/swift에서-codable-사용하기-367587c5a591

