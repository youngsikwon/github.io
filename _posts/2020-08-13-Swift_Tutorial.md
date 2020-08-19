---
layout: post
title: Swift의 기초문법 다지기!
tags: [Swift]
comments: true
---

# Swift를 배우고자 하는 초심자를 위한 가이드북입니다.

---

### 개요
<br>
> 스위프트(Swift)란?

스위프트는 애플의 IOS와 macOS 개발을 위한 프로그래밍 언어로 2014년 6월 2일 WWDC(애플 세계 개발자 회의)에서 처음으로 소개 되었습니다. 기존에 애플 운영체제 개발을 위한 Objective-c 언어가 있었지만, Swift의 약어와 철학의 운은 다음과 같습니다. 
 "신속", "빠른", "즉석" "He is Swift of foot like a horse. 또 한 다른 한편으로 개발이나 학습의 측면에서도 '빠름'을 의미 하는게 아닌가 싶습니다. 
 애플은 스위프트를 소개 할 때 가장 먼저 배우기 쉬운 강력한 프로그래밍 언어라고 설명했습니다.
 이러한 설명을 뒷받침 하듯이 애플 홈페이지(https://www.apple.com/kr/swift/)에서 대학 교육과정을 도입도 하게 되었습니다. 

스위프트는 애플의 차세데 그리고 현재의 개발자 생태계 확보를 위해 만들어진 언어라고 보여집니다.
 성능 상으로도 Objective-c를 개발하면서 흔히 발생하는 에러를 줄일 수 있도록 보완해서 스위프트를 만들었다고 하고, 스위프트로 만들어진 앱이 Objective-c로 만들어진 앱 보다 성능이 더 좋다고 애플에서 직접 발표 했다고 하니 애플에서도 개발 생태계의 흐름을 고려해 Objective-C와 스위프트은 병행 하지만 사실상 스위프트로 대체해 나가는 것 같습니다.

---


### 1. 자료형

- 변수 선언 및 할당, 할당 시 저장 및 타입 자동 설정, " 대신 '는 사용 불가

```swift
var 변수명 = "Hello World"

//출력
print(변수명) == Hello World
```

- 상수 선언 및 할당, 할당 시 이 후 변경 불가

```swift
let name="valueName"
//name = "value"로 변경불가

print(name) // reulst = valueName
```

- : 뒤에 오는 것은 타입, 따로 지정 하지 않으면 컴파일러가 알아서 설정 해주게 됨.

```swift
let name2: String = "valuename" // 문자열
let lastName: Character = "value" //문자
let name3: "name"

```
만약 타입을 따로 지정 하지 않으면 컴파일러는 문자열로 인식, 즉 문자로 인식하고 싶다면 // Character로 직접 할당해야 함.

- 정수형, 최대 최소 값은 컴퓨터의 cpu bit수에 따라 다르기에, 32bit 사용 시 64bit의 값보다 작게 나옴 Int 대신 Int32 In64로 사용 하는 것이 좋음, Int64로 선언 시 32bit cpu에서도 64bit수로 인식 하게 됨.

```swift
let age: Int = 28
let age2 = 28
let age3: Int64 = 28

print(Int.min) //reuslt = -9223372036854775808
print(Int.max) // reuslt = 9223372036854775807
```
<br>
- 실수형, 정확도를 위해 Float보다는 Double 권장

```swift
let pi = 3.141592 
let pi2: Float = 3.141592 
```
Double, 타입을 따로 지정 하지 않으면 컴파일러는 Double로 인식, 15자리

```swift
//Bolean
let isMan:Bool = true
```
---
<br>
### 기본연산자 및 사칙연산 & 논리 연산자

 Swift에서는 통상적으로 이용하는 +, -, /, % 같은 산술연산자와 &&, || 같은 논리 연산자, 그리고 C에서 지원 하지 않는 a..<b 나 a..b> 같이 값의 범위를 지정할 수 있는 범위 연산자를 지원


#### 용어
연산자에는 단항(unary), 이항(binary) 그리고 삼항(ternary) 연산자가 있습니다.

 - 단항 연산자는 - -a, !b, c! 와 같이 하나의 대상에 앞 뒤에 바로 붙여 사용하는 연산자압니다.
 - 이항 연산자는 2 + 3 같이 두 대상 사이에 위치하는 연산자입니다.
 - 삼항 연산자는 a ? b : c 형태로 Swift에 삼항 연산자는 이 연산자 단 하나만 존재합니다.



<br>
 ### 할당 연산자(Assignment Operator)

 할당 연산자는 값을 초기화 시키거나 변경합니다. 아래와 같이 상수, 변수 모두에 사용 가능합니다.

 ```swift
let b= 10
var a= 5
a = b
// a 값은 10
 ```

 아래와 같이 튜플을 이용해 여러 값을 한번에 할당할 수 있습니다.

 ```swift
let (x, y) = (1, 2)
// x는 1, y 값은 2가 됩니다.
 ```

 C나 Objective-C와 다르게 Swift에서는 할당 연산자는 값을 반환하지 않습니다.


```swift
if x = y{
    // x= y 는 값을 반환 하지 않기 때문에 이 문법은 올바르지 않습니다.
}
```
할당 연산자는 값을 반환하지 않는 이유는 동등비교연산자(==)를 사용해야 하는 곳에 할당연산자(=)가 실수로 사용 되는 것을 막기 위해서 입니다.
<br>

#### 사칙 연산자(Arithmetic Operators)

Swift는 모든 숫자 형에서 사용 가능한 4가지 표준 사칙 연산자를 지원합니다

 - 덧셈(+)
 - 뺄셈(-)
 - 곱셈(*)
 - 나눗셈(/)

```swift
let num1 = 10 + 20 //reulst 
print(num1)
let num2 = 20 - 10
print(num2)
let num3 = 10 * 2
print(num3)
let num4 = 10 / 2
print(num4)
```
C나 Objective-C와 달리 Swift는 사칙 연산의 값이 오버플로우 되는 것을 허용 하지 않습니다. 만약 이 것을 허용하고 싶으면 Swift의 오버플로우 연산자를 이용해 지원할 수 있습니다. 덧셈 연산자는 아래와 같이 문자열을 합치기 위해 사용할 수 있습니다.

```swift
"hello," + "world" // equals "hello, world" 
```
<br>
#### 나머지 연산자(Remainder Operator)

a % b 와 같이 나머지 연산을 지원합니다

```swift
9 % 4 // 1
-9 % 4 // -1
```

![img2](../img/remainderInteger_2x.png)

#### 단항 음수 연산자(Unary Minus Operator)
숫자 값은 - 로 표현되는 단항 음수 연산자에 의해 부호가 변합니다

```swift
let three = 3

let minusThree = -three // minusThree는 -3

let plusThree = -minusThree // plusThree는 3, 혹은 "minus muns 3"

```

#### 단항 양수 연산자(Unary Plus Operator)

+로 표현되는 단항 양수 연산자는 부호에 아무런 영향을 끼치지 않습니다

```swift
let minusSix = -6

let alsoMinusSix = +minusSic // alosoMinusSix는 -6
```


#### 합성 할당 연산자(Compound Assignment Operators)

a = a +2와 같이 할당연산(=)과 덧셈연산(+)으로 구성된 연산을 합성해 += 형태로 축약해 사용 가능합니다


```swift

var a = 1
a += 2 
// a는 3
```
>주의
> 합성 할당 연산자는 값을 변환하지 않습니다. 즉, let b = a+2와 같은 문법은 사용할 수 없습니다.
<br>
#### 비교 연산자(Comparison Operators)

Swift에서는 표준 C에서 제공하는 비교 연산자는 모두 지원합니다.


- 같다(a==b)
- 같지않다(a != b)
- 크다(a > b)
- 작다(a < b)
- 크거나 같다(a >= b)
- 작거나 같다 (a <= b)

> 주의
> Swift는 객체 비교르 ㄹ위해 식별 연산자 === 과 !== 를 제공합니다.

각 비교연산은 true 혹은 false을 반환합니다.
```swift
1 == 1 //true
1 != 1 //true
2 > 1 // true
1 < 2 // true
1 >= 1 //true
2 <= 1 //false
```

비교 연산은 if-else와 같은 조건 구문에서 자주 사용합니다.

```swift

let name == "World"
if name == "world" {
    print("hello, world")
} else{
    print("I'm sorry \(name), but I don't recognize you")
}
//print hello, world because name is indeed equal to "world"
```
- if문과 관련한 더 많은 정보는 [제어문 Control Flow](https://jusung.gitbook.io/the-swift-language-guide/language-guide/05-control-flow)
같은 타입의 값을 갖는 두 개의 튜플을 비교할 수 있습니다. 튜플의 비교는 왼쪽에서 오른쪽 방향으로 이뤄지고 한번에 한 개의 값만 비교 합니다. 이 비교를 다른 두 값을 비교하게 될 때까지 수행합니다.

```swift
(1, "zebra") < (2, "apple")   // true, 1이 2보다 작고; zebra가 apple은 비교하지 않기 때문
(3, "apple") < (3, "bird")    // true 왼쪽 3이 오른쪽 3과 같고; apple은 bird보다 작기 때문
(4, "dog") == (4, "dog")      // true 왼쪽 4는 오른쪽 4와 같고 왼쪽 dog는 오른쪽 dog와 같기 때문
```

>Swift 표준 라이브러리에서는 7개 요소 미만을 갖는 튜플만 비교할 수 있습니다. 만약 7개 혹은 그 이상의 요소를 갖는 튜플을 비교하고 싶으면 직접 비교 연산자를 구현해야 합니다.


#### 삼항 조건 연산자(Ternary Conditional Operator)

삼항 조건 연산자는 question ? answer1 : answer2 의 구조를 갖습니다. 그래서 question 조건이 참인 경우 answer1이 거짓인 경우 answer2가 실행됩니다.
삼항 조건 연산자는 아래 코드의 축약입니다.

```swift
if question{
    answer1
}else{
    answer2
}
```

> 삼항 조건 연산의 실제 예 입니다.

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight = contenHegit + (hasHeader ? 50 : 20)
// rowHeight는 90 (40 + 50)
```

>위에서 삼항 조건 연산을 다음과 같이 풀어 쓸 수 있습니다.


```swift
let contentHeight = 40
let hasHeader = true
let rowHeight: Int

if hasHeader{
    rowHeight = contentHeight  + 50
}else {
    rowHeight = contentHeight + 20
}
//rowHeight는 90입니다.
```

삼항 조건 연산자는 코드를 짧게 만들어 가속성을 높여줍니다.


#### Nil 병합 연산자(Nil-Coalescing Operator)

Nil 병합 연산자는 a ?? b 형태를 갖는 연산자입니다. 옵셔널 a를 벗겨서(unwraps) 만약 a가 nil인 경우 b를 반환합니다. 이 nul 병합 연산자는 다음 코드의 축약입니다.

```swift
a != nil ? a! : b
```

삼항 조건 연산자를 사용했는데, 옵셔널 a 가 nil 아니면 a를 unwrap하고 nil 이면 b를 반환하라는 의미입니다.

ex)

```swift
let defaultColorName = "red"
var userDefinedColorName: String? // 이 값은 defaults 값은 nil입니다.

var colorNameToUse = userDefinedColorName ?? defultColorName
// userDefinedColorName이 Nil이므로 colorNametoUse 값은 defaultColorname인 "red"로 설정됩니다.


userDefinedcolorName = "greed"
colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName nil이 아니므로 colorName 는 "Greed"이 됩니다.
```


#### 범위 연산자(Range Operators)

1. 닫힌 범위 연산자(Closed Range Oerators)

(a..b) 의 형태로 범위의 시작과 끝이 있는 연산자 입니다. for-in loop에 자주 사용됩니다.

```swift
for idnex in 1...5 {
print("\(index) times 5 is \(index * 5")
}

// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```
for-in loop에 관한 더 많은 정보는[제어문 Control Flow](https://jusung.gitbook.io/the-swift-language-guide/language-guide/05-control-flow)에서 보실 수 있습니다.


2. 반 닫힌 범위 연산자(Half-open Range Operator)

(a..b) 의 형태로 a 부터 b 보다 작을 때까지의 범위를 갖습니다. 즉, a 부터 b-1까지 값을 갖습니다. 이건 어디서 많이 본 범위! 그렇습니다. 보통 배열이 배열의 크기 -1의 인덱스를 갖기 때문에 이 반 닫힌 범우 ㅣ연산자는 배열을 다루는데 유용합니다.

```swift

let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("Person \(i + 1) is called \(names[i])")
}
// Person 1 is called Anna
// Person 2 is called Alex
// Person 3 is called Brian
// Person 4 is called Jack
```


3. 단방향 범위(One-Side Ranges)

[a..] [..a] 의 형태로 범위의 시작 혹은 끝만 지정해 사용하는 범위 연산자입니다. 지정한 시작 값 혹은 끝 값은 범위에 포함됩니다.

```swift
for name in names[2...]{
    print(name)
}
//Brian
//Jack


for name in names[...2]{
    prin(name)
}
//Anna
//Alex
//Brian
```

위에서 설명 했듯이 지정한 시작 혹은 끝 값이 범위에 포함된 결과입니다. 하지만 앞서 설명드린 반 닫힌 연산자는 다음과 같이 설정 값이 범위에 포함되지 않습니다.

```swift

for name in names[..<2]{
    print(name)
} 
//Anna
//Alex
```


단방향 범위 연산자는 subscript뿐만 아니라 아래와 같이 특정 값을 포함 하는지 여부를 확인할 떄도 사용 가능합니다.


```swift
let range = ...5
range.contains(7) // false
range.contains(4) //true
range.contains(-1) //true
```

#### 논리 연산자(Logical Operators)

- Swift에서는 세가지 표준 논리 연산자를 지원합니다.

 - 논리 부정 NOT(!a)
 - 논리 곱 AND(a && b)
 - 논리 합 OR (a || b)


1. 논리 부정 연산자(Logical NoT Operator)

```swift
let allowedEntry = false

if !allowebEntry {
    print("ACCESS DENIED")
}
//print "ACCESS DENIED"
```

2. 논리 곱 연산자(Logical AND Operator)

```swift
let enteredDoorCode = true
let passedRetinaScan = false
if enteredDoorCode && passedRetinaScan {
    print("Welcom!")
} else {
    print("ACCESS DENIED")
}
//prints "ACCESS DENIED"
```


3. 논리 합(OR) 연산자(Logical OR Operator)

```swift
let hasDoorKey = false
let knowsOverridePassword = true
if hasDoorKey || knowsOverridePassword{
    print("Welcom!")
}else{
    print("ACCESS DENIED")
}
// print("welcome!)
```


4. 논리 연산자의 조합(Combining Logical Operator)

두 개 이상의 논리 연산자를 조합해서 사용할 수 있습니다.

```swift
if enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "Welcome!"
```
> Swift의 논리 연산자 && 와 || 는 왼쪽의 표현을 우선해서 논리 계산을 합니다.


5. 명시적 괄호(Explicit Parentheses)

논리 연자의 적용 우선 순위를 연산자에 맡기지 않고 명시적으로 괄호를 사용해 계산 순서를 지정 할 수 있습니다.


```swift

if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// Prints "Welcome!"
```

이렇게 괄호를 사용하면 가독성시 높아져 코드의 의도를 좀 더 명확하게 전달 하는데 있어서 도움이 된다.







