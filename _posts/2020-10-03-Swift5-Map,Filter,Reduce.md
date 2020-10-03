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





