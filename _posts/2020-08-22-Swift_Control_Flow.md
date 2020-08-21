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
> Swift에서는 `while loop`, `if guard`, `Switch , for-in` 문 등 많은 제어문을 제공.

---


# For-In 문 (For-in Loops)

`for-in` 문은 배열, 숫자, 문자열을 순서대로 순회(iterate) 하기 위해 사용합니다.

```swift
let names = ["Jack", "Alex", "Youngsik"]

for name in names{
    print("Hello, \(name)")
}
 
//Hello, Jack
//Hello, Alex
//Hello, Youngsik 
```

사전(dictionary)에서 반환된 키(key)-값(value) 쌍으로 구성된 튜플을 순회하며 제어할 수도 있습니다.

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs{
    print("\(animalName)s have \(legCount) legs")
}
//ants Have 6 legs
//spiders Have 8 legs
//cats Have 4 legs`
```

사전(dictionary)에 담긴 콘텐츠는 정렬이 되지 않은 상태입니다. 사전에 넣었던 순서대로 순회 되지 않습니다. 아래와 같이 숫자 범위를 지정해 순회할 수 있습니다.

```swift
for index in 1...5{
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```

`for-in` 문을 순서대로 제어할 필요가 없다면, 변수자리에 `_` 키워드를 사용하면 성능을 높일 수 있습니다.

```swift

let base = 3
let power = 10
var answer = 1
for _ in 1...power{
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")

// Prints "3 to the power of 10 is 59049"
```

범위 연산자와 함께 사용할 수 있습니다.
```swift
let minutes = 60

for tickMark in 0..<minutes{
    // render the tick mark each minute (60 times)
}
```

`stride(from:to:by:)` 함수와 함께 사용할 수 있습니다. 다음은 구간을 5로 설정한 경우입니다.

```swift

let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval){
    // render the tic mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 50)
}
```

stride 구간을 3으로 설정한 경우
```swift

let hours = 12
let hoursInterval = 3
for tickMark in stride(from: 3, through: hours, by: hoursInterval){
    print(tickMark)
    // render the tick mark every 3 hours (3, 6, 9, 12)
}
```


<br>

# While 문 (While Loops)

Swift에서는 `while`과 `repeat-while` 두 가지 종류의 `while` 문을 지원.

## while

조건(condition)이 거짓(false)일때까지 구문(statements)을 반복합니다.

```swift
while condition{
    statements
}
```

`while`문의 (예)

```swift
var currentLevel: Int = 0, finalLvel: Int = 5
let gameCompleted = true

while (currentLevel <= finalLvel){
  //play game
  if gameCompleted{
    print("You have passed level\(currentLevel)")
    currentLevel += 1
  }
}

print("outside of while loop")
```
## Repeat-While 문

> `reapeat-while` 뭄은 다른 언어의 `do-while` 문과 유사한 `while`문입니다.

구문(statements)을 최소 한번 이상 실행하고 `while` 조건이 거짓일 떄까지 반복합니다.
```swift
repeat{
    statements
}while condition
```

`repeat-while` 문의 (예)
```swift

var currentLevel: Int = 0, finalLvel: Int = 5
let gameCompleted = true

repeat {
  //play game
  if gameCompleted{
    print("You have passed level \(currentLevel)")
    currentLevel += 1
  }
}while (currentLevel <= finalLvel)
print("outsid)
```


# 조건적 구문(Conditional Statements)

Swift에서는 `if`와 `switch` 문 두 가지의 조건 구문을 제공합니다.


## if문

`if`만 사용

```swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit  <= 32 {
  print("It's very cold, Consider waring a scarf.")
}
```

`else` 를 사용

```swift


var temperatureInFahrenheit  = 40

if temperatureInFahrenheit <= 30{
    print("It's very cold, Consider waring a scarf.")
}else{
    print("It's not that cold, war a t-shirt")
}
```


`else`, `else-if` 를 사용
```swift
var temperatureInFahrenheit = 90
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}
// Prints "It's really warm. Don't forget to wear sunscreen."
```

`else-if` 만 사용

```swift
var temperatureInFahrenheit = 72
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
}
```


## Switch

`Switch` 문의 기본 형태는 다음과 같습니다.

