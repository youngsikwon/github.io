---
layout: post
title: Swift 클로저 개념 이해(Closures)
tags: [Swift]
comments: true
---

# Swift 초심자를 위한 가이드북

---

> 클로저란?
<br>
---
클로저(Closures)는 코드블럭으로 C와 Objective-C의 블럭(Blocks)과 다른 언어의 람다(lambdas)와 비슷 합니다. 클로저는 어떤 상수나 변수의 참조를 캡처(capture)해 저장할 수 있습니다. Swift는 이 캡쳐와 관련한 모든 메모리를 알아서 처리합니다.

> 캡쳐의 개념에 대해 익숙하지 않다고 걱정 하지 않으셔도 됩니다. 값 캡쳐는 아래에서 자세히 설명 해 두었습니다.

전역 함수(Global functions)와 중첩 함수(Nested function)은 실제 클로저의 특별한 경우입니다. 클로저는 다음 세 가지 형태 중 하나만 갖습니다.

 - 전역 함수 : 이름이 있고 어떤 값도 캡쳐 하지 않는 클로저
 - 중첩 함수 : 이름이 있고 관련한 함수로부터 값을 캡쳐 할 수 있는 클로저
 - 클로저 표현 : 경량화된 문법으로 쓰여지고 관련된 문맥(context)으로부터 값을 캡쳐할 수 있는 이름이 없는 클로저
<br>

Swift에서 클로저 표현은 최적화 되어서 간결하고 명확합니다. 이 최적화에는 다음과 같은 내용을 표함합니다.

 - 문맥(context)에서 인자 타입(parameter type)과 반환 타입(return type)의 추론
 - 단일 표현 클로저에서의 암시적 반환
 - 축약된 인자 이론
 - 후위 클로저 문법

용어가 생소해 어렵게 느끼실 수 있지만, 하나하나 뜯어보면 어렵지 않고 쉽습니다. 그러니 쫄지맙시다!!

<br>

# 클로저 표현(Closure Expressions)
---
클로저 표현은 인라인 클로저를 명확하게 표현하는 방법으로 문법에 초첨이 맞춰져 있습니다. 클로저 표현은 코드의 명확성과 의도를 잃지 않으면서도 문법을 축약해 사용할 수 있는 다양한 문법의 최적화 방법을 제공합니다.

## 정렬 메소드(The Sorted method)

Swift의 표준 라이브러리에 `sorted(by:)`라는 알려진 타입의 배열 값을 정렬하는 메소드를 제공합니다. 여기 `by`에 어떤 방법으로 정렬을 수행할 것인지에 대해 기술한 클로저를 넣으면 그 방법대로 정렬된 배열을 얻을 수 있습니다. `sorted(by:)`메소드는 원본 배열은 변경 하지 않습니다. 아래와 같이 이름으로 구성된 `names` 배열을 `sorted(by:)`메소드와 클로저를 이용해 정렬 해보겠습니다.

```swift
let names = ["C", "A", "E","B","D"]
```
`sorted(by:)`메소드는 배열의 콘텐츠와 같은 타입을 갖고 두개의 인자를 갖는 클로저를 인자로 사용합니다. `names`의 콘텐츠는 String 타입이므로 `(String, String) -> Bool`의 타입으로 클로저로 사용해야 합니다.

클로저를 제공하는 일반적인 방법은 함수를 하나 만드는 것입니다. 위 타입을 만족 시키는 함수를 만들면 정렬에 인자로 넣을 수 있는 클로저를 만들 수 있습니다.

```swift
let names = ["C", "A", "E","B","D"]

func backward(_ s1: String, _ s2: String) -> Bool {
  return s1 > s2
}

var reversedNames = names.sorted(by: backward)

print("\(reversedNames)") // ["E", "D", "C", "B", "A"]
```
`backward` 클로저를 만들고 그것을 `names.sorted(by: backward)`에 넣으면 원본 배열의 순서가 바뀐 배열을 정렬 결과로 얻을 수 있습니다. 비교하는 클로저를 사용하는데 제법 긴 코드를 사용했는데, 앞으로 클로저의 다양한문법 및 사용에 대해 알아 볼 것입니다.



# 클로저 표현 문법(Closure Expression Syntax)

```swift
{ (parameters) -> return type in
    statements

}
```
인자로 넣을 `parameters`, 인자 값으로 처리할 내용을 기술하는 `statements` 그리고 `return type`입니다. 앞의 `backward` 클로저를 이용해 배열을 정렬하는 코드는 클로저 표현을 이용해 다음과 바꿀 수 있습니다.

```swift
reverseNames = names.sorted(by: {(s1: String, s2: String) -> Bool in
    return s1 > s2
})
```
이렇게 함수로 따로 정의된 형태가 아닌 인자로 들어가 있는 형태의 클로저를 `인라인 클로저`라 부릅니다. 클로저의 몸통(body)는 `in` 키워드 다음에 시작합니다. 사용할 인자 값과(parameters) 반환 타입(return type)을 알았으니 이제 그것들을 적절히 처리해 넘겨 줄 수 있다는 뜻입ㅂ니다. 클로저의 `body`가 짧으니  아래와 같이 한줄에 적을 수 있다.

```swift
reverseNames = names.sorted(by: {( s1: String, s2: tring) -> Bool in return s1 > s2})
```

## 문맥에서 타입 추론(Inferring Type From COntext)

위 예제에서 정렬 클로저는 String 배열에서 `sorted(by:)` 메소드의 인자로 사용됩니다. `sorted(by:)`의 메소드에서 이미 `(string, string) -> Bool` 타입의 인자가 들어와야 하는지 알기 떄문에 클로저에서 이 타입들을 생략 될 수 있습니다. 그래서 위 함수는 더 생략한 형태로 아래와 같이 기술 할 수 있습니다.

```swift
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
```
위와 같이 클로저의 인자 값과 반환 타입을 생략할 수 있지만, 가독성과 코드의 모호성을 피하기 위해 타입을 명시할 수도 있습니다.

## 단일 표현 클로저에서의 암시적 반환 (Implicit Returns from Single-Express Closures)

단일 표현 클로저에서는 반환 키워드를 생략할 수 있습니다.

```swift
  reversedNames = names.sorted(by: {s1, s2 in s1 > s2 })
```
> 이렇게 표현해도 어떠한 모호성도 없습니다. s1과 s2를 인자로 받아 그 두 값을 비교한 결과를 반환합니다.

## 인자 이름 축약(Shorthand Arguments Names)

Swift는 인라인 클로저에 자동으로 축약 인자 이름을 제공합니다. 이 인자를 사용하면 인자 값을 순서대로 $0, $1, $2 등으로 사용할 수 있습니다. 축약 인자 이름을 사용하면 인자 값과 그 인자로 처리할 때 사용하는 인자가 같다는 것을 알기 떄문에 인자를 입력 받는 부분과 `in` 키워드 부분을 생략 할 수 있습니다. 그러면 이렁 형태로 축약 가능합니다.

```swift
  reversedNames = names.sorted(by: { $0 > $1 })
```
축약은 되었지만 논리를 표현하는데는 지장이 없습니다. 인라인 클로저에 생략된 내용을 포함해 설명하면 
 - $0과 $1 인자를 두개 받아서 $0dl $1 보다 큰지를 비교하고, 그 결과 (Bool)를 반환.

 ## 연산자 메소드(Operator Methods)

 축약이 이게 끝인줄 아셨겠지만 아닙니다. 여기서 더 줄일 수 있습니다. Swift의 `String` 타입 연산자에는 `String` 끼리 비교할 수 있는 비교 연산자 (>)를 구현해 두었습니다. 이 때문에 그냥 이 연산자를 사용하면 됩니다.

```swift
    reversedNames = names.sorted(by: >)
```


## 후위 클로저(Trailing Closures)
---
만약 함수의 마지막 인자로 클로저를 넣고 그 클로저가 길다면 후위 클로저를 사용할 수 있습니다. 이런 형태의 클로저가 있다면

```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // function body goes here
}
```
위 클로저의 인자 값 입력 부분과 반환 형 부분을 생략해 다음과 같이 표현할 수 있고

```swift
someFunctionThatTakesAClosure(closure: {
    //closur's body goes here
})
```
이것을 후위 클로저로 표현하면 아래와 같이 표현할 수 있습니다. 함수를 대괄호 ({,})로 묶어 이 그 안에 처리할 내용을 적으면 됩니다. 모르고 사용하셨다면 이런 일반적인 전역함수 형태가 사실 클로저를 사용하고 있떤 것이었습니다.

```swift
someFunctionThatTakesAClosure(){
    // trailing closure's body goed here
}
```
앞의 정렬 예제를 후위 클로저를 이용해 표현하면 이렇게 표현할 수 있습니다.
```swift
reverseNames = names.sorted( {$0 > $1})
```
만약 이 함수의 마지막 인자가 클로저이고 후위 클로저를 사용하면 괄호 `()`를 생략할 수 있습니다.

```swift
reverseNames = names.sorted{$0 > $1}
```

이번에는 후위 클로저를 이용해 숫자(Int)를 문자(String)로 매핑(Matting) 하는 예제를 살펴 보겠습니다.
```swift
let digitNames = [
    0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]
let numbers = [16, 58, 510]
```

이 값을 배열의 `map(_:)` 메소드를 이용해 특정 값으로 매핑하는 할 수 있는 클로저를 구현합니다.

```swift
let strings = numbers.map { (number) -> String in
    var number = number
    var output = ""
    repeat {
        output = digitNames[number % 10]! + output
        number /= 10
    } while number > 0
    return output
}
// let strings는 타입 추론에 의해 문자 배열([String])타입을 갖습니다.
// 결과는 숫자가 문자로 바뀐 ["OneSix", "FiveEight", "FiveOneZero"]가 됩니다.
```
위 코드는 각 자리수를 구해서 그 자리수를 문자로 변환하고, 10으로 나눠서 자리수를 바꾸며 문자로 변환하는 것을 반복합니다. 이 과정을 통해 숫자 배열을, 문자 배열로 바꿀 수 있습니다.
`number`값은 상수인데, 이 상수 값을 클로저 안에서 변수`var`로 재정의 했기 때문에 `number` 값의 변환이 가능합니다. 기본적으로 함수와 클로저에 넘겨지는 `인자 값`은 `상수`입니다.

> `digitNames[numver % 10]!`에 뒤에는 느낌표(!)가 붙어 있는 것은 사전(dictionary)의 `subscript`는 옵셔널이기 때문에 즉, 사전에서 특정 `key`에 대한 값은 있을 수도 있고 없을 수도 있기 때문에 논리적으로 당연한 일입니다.

# 값 캡쳐(Capturing values)
---

클로저는 특정 문맥의 상수나 변수의 값을 캡쳐할 수 있습니다. 다시 말해 원본 값이 사라져도 클로저의 `body`안에서 그 값을 활용할 수 있습니다. Swift에서 값을 캡처 하는 가장 단순한 형태는 `중첩 함수(nested function)` 입니다. 중첩 함수는 함수의 body에서 다른 함수를 다시 호출하는 형태로 된 함수 입니다. 

```swift
func makeIncrementer(forIncrement amout: Int) -> () -> Int {
  var runningTotal = 0

  func increment() -> Int {
    runningTotal += amount
    return runningTotal
  }
  return increment
}
```
이 함수는 `makeIncrementer` 함수 안에서 `incrementer`함수를 호출하는 형태로 중첩 함수입니다. 클로저의 인자와 반환 값이 보통의 경우와 달라 어렵게 보일 수 있는데, 이것도 역시 쪼개 보면 어렵지 않습니다.인자와 반환 값 `(forIncrement amount: Int) -> () -> Int` 중에 처음 -> 를 기준으로 앞의 `(forIncrement amount: Int)` 부분이 인자 값이고 뒤 () -> Int는 반환 값입니다. 그렇습니다. 이것은 반환 값이 클로저인 형태입니다. 반환 값을 인자가 없고 Int형의 클로저를 반환한다는 의미입니다. 함수 안의 `incrementer` 함수만 따로 보겠습니다.

```swift
func incrementer() -> Int {
    runningTotal += amount
    return runningTotal
}
```

`runningTotal`과 `amount`도 없습니다. 하지만 이 함수는 돌아갑니다. 그것은 `runningTotal`과 `amount`가 캡쳐링 되서 그런 것입니다.

> 최적화 이유로 Swift는 만약 더 이상 클로저에 의해 값이 사용되지 않으면 그 값을 복사해 저장하거나 캡쳐링 하지 않습니다. Swift는 또 특정 변수가 더 이상 필요 하지 않을 때 제거 하는 것과 관련한 모든 메모리 관리를 알아서 처리 합니다.

<br>

이제 위 중첩 함수를 실행해 보겠습니다.

```swift
let incrementByTen = makeIncrementer(forIncrement: 10)
```

`makeIncrementer` 함수는 클로저를 반환합니다. 여기서는 `makeIncrementer` 내부의 `increment` 함수를 실행하는 메소드 반환.

```swift
incrementByTen()
// 값으로 10을 반환

incrementByTen()
// 값으로 20을 반환

incrementByTen()
// 값으로 30을 반환
```
함수가 각기 실행 되지만 실제로는 변수 `runningTotal`과 `amount`를 캡쳐링 되서 그 변수를 공유하기 때문에 계산이 누적된 결과를 갖습니다. 만약 아래와 같이 새로운 클로저 생성하면 어떻까요?

```swift
let incrementBySeven = makeIncrementer(forIncrement: 7)
incrementBySeven
//return a value of 7
```
네, 다른 클로저이기 때문에 고유의 저장소에 `runningTotal`과 `amount`를 캡쳐링 해서 사용합니다. 그래서 다른 값이 나옵니다. 그렇다면 여기서 이전의 클로져를 실행하면 어떻게 될까요?

```swift
increment()
// 40반환
```

네. 다른 클로저이기 때문에 연산에 전혀 영향이 없습니다. 그냥 다른 저장소의 사용해 계산합니다.

> 만약 클로저를 어떤 클래스 인스턴스의 프로퍼티로 할당하고, 그 클로저가 그 인스턴스에 캡쳐링하면 강한 순환참조에 빠지게 됩니다. 즉, 인스턴스의 사용이 끝나도 메모리를 해제 하지 못하는 것이죠. 그래서 Swift는 이 문제를 다루기 위해 캡쳐 리스트(Capture list)를 사용합니다.

# 클로저는 참조 타입(Closure Are Reference Types)

앞의 예제에서 `incrementBySeven`과 `IncrementByTen`는 상수입니다. 그런데 어떻게 `runningTotal`변수를 계속 증가 시킬 수 있는 걸까요? 답은 함수와 클로저는 참조 타입이기 떄문입니다. 함수와 클로저를 상수나 변수에 할당할 때 실제로는 상수와 변수에 해당 함수나 클로저의 참조(Reference)가 할당 됩니다. 그래서 만약 한 클로저를 두 상수나 변수에 할당하면 그 두 상수나 변수는 같은 클로저를 참조하고 있습니다. C 나 C++에 익숙하신 분은 함수 퐁니터를 저장한다고 생각하시면 이해하기 쉽다.

```swift
let alsoIncrementByTen = incrementByTen
alsoIncrementBtTen()
// 50을 반환
```
앞에서 사용했던 클로저를 상수에 할당하고 실행시키면 사용한 클로저의 마지막 상태에서 10을 증가시켜 결과 값으로 50을 반환하게 됩니다.


# 이스케이핑 클로저(Escaping Closures)
---
클로저를 함수의 파라미터로 넣을 수 있는데, 함수 밖(함수가 끝나고)에서 실행되는 클로저 예를들어, 비동기로 실행되거나 `completionHandler`로 사용되는 클로저는 파라미터 타입 앞에 `@escaping`이라는 키워드를 명시 해야한다.

```swift
var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
```

위 함수에서 인자로 전달된 `completionHandler`는 `someFunctionWithEscapingClosure` 함수가 끝나고 나중에 처리 됩니다. 만약 함수가 끝나고 실행되는 클로저에 `@escaping` 키워드를 붙이지 않으면 컴파일시 오류가 발생합니다.

`@escaping` 를 사용하는 클로저에서는 `self`를 명시적으로 언급해야 합니다.
```swift
func someFunctionWithNonescapingClosure(closure: () -> Void) {
    closure()    // 함수 안에서 끝나는 클로저
}

class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { self.x = 100 } // 명시적으로 self를 적어줘야 합니다.
        someFunctionWithNonescapingClosure { x = 200 }
    }
}

let instance = SomeClass()
instance.doSomething()
print(instance.x)
// Prints "200"

completionHandlers.first?()
print(instance.x)
// Prints "100"
```



---

# 자동클로저(Autoclosures)

자동클로저는 인자 값이 없으며 특정 표현을 감싸서 다른 함수에 전달 인자로 사용할 수 있는 클로저입니다. 자동클로저는 클로저를 실행하기 전까지 실제 실행이 되지 않습니다. 그래서 계산이 복잡한 연산을 하는데 유용합니다. 왜냐면 실제 계산이 필요할 때 호출되기 때문입니다. 예제를 보면서 무슨 뜻인지 알아 보겠습니다.

```swift
// customersInLine is ["Alex", "Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: { customersInLine.remove(at: 0) } )
// Prints "Now serving Alex!"
```
`serve`함수는 인자로 `() -> String)` 형, 즉 인자가 없고, `String`을 반환하는 클로저를 받는 함수 입니다. 그리고 이 함수를 실행할 때는 `serve(customer: { customersInLine.remove(at: 0) } )`이와 같이 클로저`{ customersInLine.remove(at: 0) }`를 명시적으로 직접 넣을 수 있습니다.

위 예제에서는 함수의 인자로 클로저를 넣을 때 명시적으로 넣는 경우에 대해 알아 보았습니다. 위 예제를 `@autoclosure`키워드를 이용해서 보다 간결하게 사용할 수 있습니다. 예제를 보시겠습니다.

```swift
// customersInLine is ["Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: @autoclosure () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: customersInLine.remove(at: 0))
// Prints "Now serving Ewa!"
```
`serve`함수의 인자를 받는 부분 `customerProvider: @autoclosure()` 에서 클로자의 인자 () 앞에 `@autoclosure` 라는 키워드를 붙였습니다. 이 키워드를 붙임으로써 인자 값은 자동으로 클로저로 변환됩니다. 그래서 함수의 인자 값을 넣을 때 클로저가 아니라 클로저가 반환하는 반환 값과 일치하는 형의 함수를 인자로 넣을 수 있습니다. 그래서 `serve(customer: { customersInLine.remove(at: 0) } )` 이런 코드를 `@autoclosure`키워 `@autoclosure`를 선언하면 함수가 이미 클로저 인것을 알기 떄문에 리턴값 타입과 같은 값을 넣어줄 수 있습니다.

> NOTE 자동클로저를 너무 남용하면 코드를 이해하기 어려워 질 수 있습니다. 그래서 문맥과 함수 이름이 `autoclosure`를 사용하기에 분명해야 합니다.

자동클로저 `@auclosure`는 이스케이프 `ascaping`와 같이 사용 할 수 있습니다.
```swift
// customersInLine is ["Barry", "Daniella"]
var customerProviders: [() -> String] = []        //  클로저를 저장하는 배열을 선언
func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
    customerProviders.append(customerProvider)
} // 클로저를 인자로 받아 그 클로저를 customerProviders 배열에 추가하는 함수를 선언
collectCustomerProviders(customersInLine.remove(at: 0))    // 클로저를 customerProviders 배열에 추가
collectCustomerProviders(customersInLine.remove(at: 0))

print("Collected \(customerProviders.count) closures.")
// Prints "Collected 2 closures."        // 2개의 클로저가 추가 됨
for customerProvider in customerProviders {
    print("Now serving \(customerProvider())!")    // 클로저를 실행하면 배열의 0번째 원소를 제거하며 그 값을 출력
}
// Prints "Now serving Barry!"
// Prints "Now serving Daniella!"
```
`collectCustomerProviders`함수의 인자 `customerProvider`는 `@autoclosure`이면서 `@escaping`로 선언되었습니다. `@autoclosure`로 선언됐기 때문에 함수의 인자로 리턴값 `String`만 만족하는 `customersInLine.remove(at: 0)`형태로 함수 인자에 넣을 수 있고, 이 클로저는 `collectCustomerProviders`함수가 종료된 후에 실행되는 클로저 이기 때문에 인자 앞에 `@escaping` 키워드를 붙여주었습니다.






