---
title: "[Swift] Argument label? Parameter name?"

categories:
  - Swift
tags:
  - [Swift, Argument label, Parameter name]

date: 2023-03-23
last_modified_at: 2023-03-23
---

Swift에서 함수를 사용하다보면 **argument label**과 **parameter name**이 혼동될 때가 있다. 
이 참에 확실히 정리해보겠다.
## • Argument, Parameter 그게 뭔데?
다음은 Swift에서의 함수 예시이다.

```swift
func plus(a: Int, b: Int) -> Int {
    return a + b
}

plus(4, 6)
```
함수를 정의할 때 사용되는 변수를 **Parameter**(매개변수),
실제로 함수가 호출될 때 넘기는 변수값을 **Argument**(인수) 라고 한다.
위 예시에서 Parameter는 `a`, `b`가, Argument는 `4`, `6`이 된다.

## •Argument label과 Parameter name
Swift에서는 함수를 정의할 때 함수 사용자의 입장에서 매개변수의 역할을 좀 더 명확하게 표현하고자 할 때 **Argument label**(인자이름)을 사용할 수 있다.

```swift
func 함수명(인자이름 매개변수: 인자타입) -> (반환 타입) {
  var result = 매개변수
  return result
}

함수명(인자이름: 15)
```

```swift
func convertToString(from numbers: Set<Int>) -> String {
    let result = numbers.map { String($0) }.joined(separator: ", ")
    
    return result
}

let exampleNumbers: Set<Int> = [2, 4, 1, 6, 5]
convertToString(from: exampleNumbers) // "2, 4, 1, 6, 5"
```
위 예시에서 **Parameter name**은 `numbers`가, **Argument label**은 `from`이 된다.

## • 그럼 이건?
다음 예시에서 Parameter name과 Argument label은 각각 무엇일까?

```swift
func greet(person: String) {
    let greeting = "Hello, " + person + "!"
    print(greeting)
}

greet(person: "Jimmy")    // "Hello, Jimmy!"
```
Parameter name과 Argument label이 구별되어 있지 않은 경우엔 **동일한 의미**가 된다.
즉, 모두 `person`을 의미한다.

>[_By default, parameters use their parameter name as their argument label._](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/functions/#:~:text=By%20default%2C%20parameters%20use%20their%20parameter%20name%20as%20their%20argument%20label.)

## • 번외
함수를 호출할 때 Argument label을 쓰는 것이 번거로울 경우도 있습니다.
이 때는 `_`를 통해 인자이름을 생략하여 쓸 수 있습니다.

```swift
func orderTo(_ coffee: String) {
    print("주문하신 \(coffee) 나왔습니다.")
}

orderTo("caffeLatte")    // "주문하신 caffeLatte 나왔습니다."
```


