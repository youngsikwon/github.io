
---
layout: post
title: Swift 제어문(Control Flow)
tags: [Swift]
comments: true
---

# Swift를 배우고자 하는 초심자를 위한 가이드북입니다.

---

### 개요
<br>
> 함수(Functions)

---
## 정의와 호출(Defining and Calling Functions)

함수를 선언할 떄는 가장 앞에 `func`키워드를 붙이고 `(person: String)` 파라미터와 형 그리고 `->String` 형태로 반환형을 정의합니다.

```swift
func greet(person: String) -> String {
 let greeting = "Hello, " + person + "!"
 return greeting
}
```

정의한 함수에 인자 값을 넣어 호출한 (예)
```swift
print(greet(person: "Anna"))
//reulst : Hello, Anna!

//예) print(greet(person: "문장 입력대로 출력 됨."))
```
위 함수에서 메세지를 결합하는 부분과 반환하는 부분을 합쳐서 더 짧게 만들 수 있습니다.
```swift
func greetAgain(person: String) -> String {
    return "Hello again, " + person + "!"
}
print(greetAgain(person: "Anna"))
// Prints "Hello again, Anna!"
```

# 함수 파라미터와 반환 값(Function Parameters and Return Values)
---

## 파리미터가 없는 함수(Functions Without Parameters)

```swift
func sayHelloWorld() -> String{
  return "hello world"
}
print(sayHelloWorld())
```

## 복수의 파라미터를 사용하는 함수(Functions With Multiple Parameters)

```swift
func greet(person: String, alreadyGreeted: Bool) -> String {
    if alreadyGreeted {
        return greetAgain(person: person)
    } else {
        return greet(person: person)
    }
}
print(greet(person: "Tim", alreadyGreeted: true))
// Prints "Hello again, Tim!"
```

## 반환 값이 없는 함수(Functions Without Return Values)

```swift
func greet(person: String){
    print("Hello \(person)")
}
greet(person: "Dave")
```
> 주의
> 엄밀히 말하면 위 함수는 반환 값을 선언하지 않았지만 반환 값이 있습니다. 반환 값이 정의 되지 않은 함수는 Void라는 특별한 형을 반환합니다. Void는 간단히 ()를 사용한 빈 튜플입니다.

함수의 반환 값은 아래와 같이 호출 될때 무시될 수 있다.
```swift
func printAndCount(string: String) -> Int {
    print(string)
    return string.count
}
func printWithoutCounting(string: String) {
    let _ = printAndCount(string: string)
}
printAndCount(string: "hello, world")
// prints "hello, world" and returns a value of 12
printWithoutCounting(string: "hello, world")
// prints "hello, world" but does not return a value
```




