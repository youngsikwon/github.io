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

```swift
switch some value to consider{
    case value 1:
        respond to value 1
    case value 2, value 3:
        respond to value 2 or 3
    default:
        otherwise, do something else
}
```


문자를 비교해 처리하는 경우 아래와 같이 사용할 수 있습니다.

```swift

let someCharacter: Character = "z"
switch someCharacter {
    case "a":
        print("The first letter of the alphabet")
    case "z":
        print("The last letter of the alphabet")
    default:
        print("Some other character")
}
//Prints `The last letter of the alphabet'
```
## 암시적인 진행을 사용 하지 않음 (No Implicit Fallthrough)

C와 Objective-c의 `switch` 구문과는 달리 Swift의 `switch` 구문은 암시적인 진행을 하지 않습니다. C나 Objective-c에서는 `switch` 구문이 기본적으로 모든 `case`를 순회하여 `default`를 만날 때까지 진행됩니다. 그래서 그것을 진행하지 않기 위해 `break` 라는 문구를 명시적으로 적어야 했습니다. Swift에서는 `break` 를 적지 않아도 특정 `case`가 완료 되면 자동으로 `switch` 구문을 빠져 나오게 됩니다. 이런 사용 법으로 인해 실수로 `break`를 적지 않아 의도하지 않은 `case`문이 실행 되는 것을 방지 해줍니다.


> 주의
> `break`가 Swift에서 필수적이지 않지만 `case`안에 특정 지점에서 멈추도록 하기 위해 `break` 사용.

<br>

`case` 안 에 최소 하나의 실행 구문이 반드시 있어야 합니다.

```swift
let anotherCharacter: Character = "a"

switch anotherCharacter{
    case "a":
    case "A":
        print("the letter A")
    default:
        print("Not the letter A")
}
//컴파일 에러 발생!
```

case 안에 콤마(,)로 구분해서 복수의 case 조건을 혼합(compound)해 사용할 수 있습니다.

```swift
let anotherCharacter: CCharacter = "a"

switch anotherCharacter{
    case "a","A":
        print("The letter A")
    default:
        print("Not the letter A")
}
//prints the letter A
```

가독성 떄문에 혼합해 사용하는 경우를 여러 코드라인에 나눠서 적을 수 있습니다. 더 많은 정보는 혼합 케이스(Compound  Cases)에서 확인할 수 있습니다.

> 주의
> 명시적으로 `switch-case` 문의 특정 지점의 끝까지 실행하고 싶다면 `fallthrough` 키워드를 사용할 수 있습니다.


## 인터벌 매칭(Interval Matching)

숫자의 특정 범위를 조건으로 사용할 수 있습니다.

