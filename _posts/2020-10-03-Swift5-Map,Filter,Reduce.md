---
layout: post
title: Swift5 - Map, Filter, Reduce
tags: [Swift]
comments: true
---


# 맵, 필터, 리듀스

- 스위프트는 함수를 일급 객체로 취급합니다. 따라서 함수를 다른 함수의 전달인자로 사용할 수 있습니다. 매개변수로 함수를 갖는 함수를 고챃마수라고 부르는데, 스위프트에 유용한 대표적인 고챃마수로는 `맵`, `필터`, `리듀스` 등이 있습니다.



## Map

> `Map`은 자신을 호출할 때 매개변수로 전달된 함수를 실행하여 그 결과를 다시 반환 해주는 함수입니다. 스위프트에서 맵은 `배열, 딕셔너리, 세트, 옵셔널` 등에서 사용할 수 있습니다. 조금 더 정확히 말하자면 스위프트의 `Sequence, Collection` 프로토콜을 따르는 타입과 옵셔널은 모두 맵을 사용할 수 있습니다.

맵을 사용하면 컨테이너가 담고 있던 각각의 값을 매개변수를 통해 받은 함수에 적용한 후 다시 컨테이너에 포장하여 반환됩니다. 기존 컨테이너의 값은 변경되지 않고 새로운 컨테이너가 생성되어 반환됩니다. 그래서 `맵은 기존 데이터 변형` 하는 데 많이 사용 합니다.


- 맵 매서드의 호출 및 결과

```swift

container.map(f(x)) // 컨테이너의 map 매서드 호출

return f(container의 각 요소)
           //새로운 컨테이너

```

`map` 메서드의 사용법은 앞서 알아본 `for-in` 구문과 별반 차이가 없습니다. 다만 코드의 재사용 측면이나 컴파일러 최적화 측면에서 본다면 성능 차이가 있습니다.
또, 다중 스레드 환경일 때 대상 컨테이너의 값이 스레드에서 변경되는 시점에 다른 스레드에서도 동시에 값이 변경되려고 할 때 예측치 못한 결과가 발생하는 부작용을 방지할 수도 있습니다.


```swift
let numbers: [Int] = [0, 1, 2, 3, 4]

var doubleNumbers: [Int] = [Int]()
var strings: [String] = [String]()

//for 구문 사용
for number in numbers{
  doubleNumbers.append(number * 2)
  strings.append("\(number)")
}

print(doubleNumbers) // 0 2 4 6 8
print(strings) // 0, 1, 2, 3, 4



// map 메서드 사용

doubleNumbers = numbers.map({ (number: Int) -> String in 
  return "\(number)"})

print(doubleNumbers)// [0, 2, 4, 6, 8]
print(strings)// ["0", "1", "2", "3", "4"]
```

짧은 코드라 차이를 느끼기는 힘들겠지만, 이 코드를 살펴보면 `map` 메서드를 사용했을 때 `for-in` 구문을 사용하기 위하여 빈 배열을 처음 생성해주는 작업도 필요도 없습니다.
배열의 append 연산을 실행하기 위한 시간도 필요 없다.


`map` 메서드를 사용하여 코드가 조금 더 간략해지지 했지만, 우리가 배웠던 클로저 표현식을 사용하여 표현을 더 간략화해볼 수 있습니다. 

> 클로저 표현의 간략화

```swift
let numbers: [Int]  = [0, 1, 2, 3, 4]


//기본 클로저 표현
var doubleNumbers = numbers.map({ (number: Int) -> Int in return number * 2
})

//매개변수 및 반환 타입 생략
doubleNumbers = numbers.map({return $0 * 2 })
print(doubleNumbers) //[0, 2, 4, 6, 8]

//반환 키워드 생략
doubleNumbers = numbers.map({ $0 * 2 })
print(doubleNumbers)//[0, 2, 4, 6, 8]

// 후행 클로저 사용
doubleNumbers = numbers.map {$0 * 2}
print(doubleNumbers)//[0, 2, 4, 6, 8]

```



## 리듀스

 - 리듀스 기능은 `결합`이라고 불러야 마땅합니다. 컨테이너 내부의 콘텐츠를 하나로 합하는 기능을 실행하는 고차 함수입니다. 배열이라면 배열의 모든 값을 전달인자로 전달받은 클로저의 연산 결과를 합해줍니다.

 - Swift의 리듀스는 두 가지 형태로 구현되어 있습니다.
  
  1. 클로저가 각 요소를 전달받아 연산한 후 값을 다음 클로저 실행을 위해 반환하며 컨테이너를 순환하는 형태입니다.



```swift
public func reduce<Result>(_ initialResult: Result,
_ nextPartialResult: (Result, Element) throws -> Result) rethrows -> Result

```
- 풀이

 - initialResult 매개변수로 전달되는 값을 통해 초깃값을 지정해 줄 수 있으며, nextPartialResult라는 매개변수로 클로저 값으 받습니다. nextPartialResult 클로저의 첫 번재 매개변수는 리듀스 메서드의 initialResult 매개변수를 통해 전달받은 초깃값 또는 이전 클로저의 결괏값.


```swift
public func reduce<Result>(into initialResult: Result,
_ updateAccumulationgResult: (inout Result, Element) throws -> ()) rethrows -> Result
```

- 리듀스의 메서드 사용

```swift


let numbers: [Int] = [1, 2, 3]

var sum: Int = numbers.reduce(0, { (result: Int, next: Int) -> Int in 
print("\(result) + \(next)")

return result + next 
})


print(sum) //6
print("=====================================")
//초깃값이 0이고 정수 배열의 모든 값을 뻅니다.

var subtract: Int = numbers.reduce(0, { (result: Int, next: Int) -> Int in
print("\(result) - \(next)")

return result - next
})

print(subtract)// -6

print("=====================================")

// 초깃값이 3이고 정수 배열의 모든 값을 뻅니다.
var subractFromThree: Int = numbers.reduce(3){
  print("\($0) = \($1)")

  return $0 - $1
}
print(subractFromThree)
print("=====================================")


// 문자열 배열을 reduct(_:-:) 메서드를 이용해 연결시킵니다.

let names: [String] = ["Chope", "Jay", "Joker", "Nova"]

let reductNames: String = names.reduce("youngsik's friend:") {
  return $0 + "," + $1
}
print(reductNames)

print("=====================================")

// 두 번째 형태인 reduct(into:_:) 메서드의 사용

// 초깃값이 0이고 정수 배열의 모든 값을 더합니다.
//첫 번째 리듀스 형태와 달리 클로저의 값을 반환하지 않고 내부에서
//직접 이전 값을 변경한다는 점이 다릅니다.
sum = numbers.reduce(into: 0, { (result: inout Int, next: Int) in
print("\(result) + \(next)")

result += next
})

print(sum) //6

print("=====================================")
// 초깃값이 3이고 정수 배열의 모든 값을 뻅니다.
// 첫 번쨰 리듀스 형태와 달리 클로저의 값을 반환하지 않고 내부에서
//직접 이전 값을 변경한다는 점이 다릅니다.

subractFromThree = numbers.reduce(into: 3, {
  print("\($0) - \($1)")
})

print(subractFromThree) // -3


print("=====================================")

//첫 번째 리듀스 형태와 다르기 떄문에 다른 컨테이너에 값을 변겨앟여 넣어줄 수도 있습니다.
//이렇게 하면 맵이나 필터와 유사한 형태로 사용할 수 있습니다.
//홀수로 걸러내고 짝수만 두 배로 변경하여 초깃값만 [1, 2, 3] 배열에 직접 연산합니다.
var doubleNames: [Int] = numbers.reduce(into: [1,2]){ (result: inout [Int], next: Int) in
print("result: \(result) next: \(next)")

guard next.is else{
  return
}

print("\(result) append \(next)")

result.append(next * 2)

}

print(doubleNames) // [1], [2], [3]


//  필터와 맵을 사용한 모습
doubleNames = [1,2] + numbers.filter { $0.isMultiple(of: 2) }.map { $0 * 2}
print(doubleNames) // [1, 2, 4]


```
---





