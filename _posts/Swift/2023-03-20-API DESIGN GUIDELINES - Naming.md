---
title: "[Swift] API DESIGN GUIDELINES - Naming"

categories:
  - Swift
tags:
  - [Swift, API DESIGN GUIDELINES]

toc: true
toc_sticky: true

date: 2023-03-20
last_modified_at: 2023-03-20
---

# [Swift] API DESIGN GUIDELINES - Naming

[Swift API DESIGN GUIDELINES](https://www.swift.org/documentation/api-design-guidelines/#naming) 참고

> - Promote Clear Usage (명확하게 사용하기)
> - Strive for Fluent Usage (자연스러운 사용을 위해 노력하기)
> - Use Terminology Well (적절한 기술 용어 사용하기)

## • Promote Clear Usage (명확하게 사용하기)
- **Include all the words needed to avoid ambiguity**
(모호함을 피하기 위해 모든 단어를 포함한다.)
```swift
✅
extension List {
  public mutating func remove(at position: Index) -> Element
}
employees.remove(at: x)
```
```swift
⛔️
employees.remove(x) // unclear: are we removing x?
```
`at` 이라는 단어를 생략한다면 x의 쓰임을 분명하게 알 수 없다.

- **Omit needless words**
(불필요한 단어는 생락한다.)
```swift
⛔️
public mutating func removeElement(_ member: Element) -> Element?

allViews.removeElement(cancelButton)
```
```swift
✅
public mutating func remove(_ member: Element) -> Element?

allViews.remove(cancelButton) // clearer
```
분명한 의도를 위해 더 많은 단어가 필요할 수 있지만, 읽는 사람이 이미 알고 있는 **중복**되는 정보는 생략한다.
```swift
⛔️
func setIntToString(setInt: Set<Int>) -> String {
    let setInt = setInt
    let setString = setInt.map {String($0)}
    let string = setString.joined(separator: ", ")

    return string
}
```
```swift
✅
func convertToString(from numbers: Set<Int>) -> String {
    let result = numbers.map {String($0)}.joined(separator: ", ")
    
    return result
}
```
특히, **타입** 정보만 반복하는 단어는 생략한다.
타입 제약이 아닌 **해당 역할**에 따라 변수, 매개변수 및 연관 타입을 명명한다.
## • Strive for Fluent Usage (자연스러운 사용을 위해 노력하기)
- **Prefer method and function names that make use sites form grammatical English phrases.**
(사용되는 메서드와 함수의 이름은 영어 문법에 맞는 구가 되는 것이 좋다.)
```swift
✅
x.insert(y, at: z)          “x, insert y at z”
x.subViews(havingColor: y)  “x's subviews having color y”
x.capitalizingNouns()       “x, capitalizing nouns”
```
```swift
⛔️
x.insert(y, position: z)
x.subViews(color: y)
x.nounCapitalize()
```
- **Begin names of factory methods with “make”**, e.g. x.makeIterator().
(x.makeIterator()와 같이 팩토리 메서드는 "make"로 시작한다.)
- **Name functions and methods according to their side-effects**
  (함수와 메서드는 그 부수 효과에 따라서 명명한다.)
  **	- without side-effects**, e.g. x.distance(to: y), i.successor()
  부수 효과가 없는 함수 및 메서드는 **명사 구**로 읽는다.
  **  - with side-effects**, e.g., print(x), x.sort(), x.append(y)
  부수 효과가 있는 함수 및 메서드는 명령형인 **동사 구**로 읽는다.
  
## • Use Terminology Well (적절한 기술 용어 사용하기)
- **Avoid obscure terms**
(이해하기 힘든 용어는 피한다.)
Don’t say “epidermis” if “skin” will serve your purpose.
"피부" 로 의미가 전달된다면 "표피" 로 말하지 않는다.
- **Avoid abbreviations.**
(축약어는 피한다.)
```swift
⛔️
var randomNum: Int
var resultArr: [String]
```
```swift
✅
var randomNumber: Int
var resultArray: [String]
```
- **Embrace precedent**
(선례를 따르도록 한다.)
It is better to name a contiguous data structure `Array` than to use a simplified term such as `List`.
연속적인 자료구조의 이름으로 `List`보다는 `Array`를 사용하는 것이 좋다.