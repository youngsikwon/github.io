---
layout: post
title: Swift5 - Monad
tags: [Swift]
comments: true
---


모나드(Monad)는 순서가 있는 연산을 처리하는데 사용하는 디자인 패턴이다. 모나드는 순수 함수형 프로그래밍 언어에서 부작용을 관리하기 위해 광범위하게 사용되며 복합 체계 언어에서도 복잡도를 제어하기 위해 사용된다.




## Monad 갖춰야하는 조건 다음과 같습니다.

- 타입을 인자로 받는 타입(특정 타입의 값을 포장)
- 특정 타입의 값을 포잫안 것을 반환하는 함수(메서드)가 존재
- 포장된 값을 변환하여 같은 형태로 포장하는 함수(메서드)가 존재

```swift
func doubleEven(_ num: Int) ->  Int? {
  if num.isMultiple(of: 2) {
    return num * 2
  }
  return nil
}

Optional(3).flatMap(doubleEven)
```


## 컨텍스트

컨텍스트의 사전적 정의를 보면 `맥락`, `전후 사정` 등입니다. `콘첸츠`를 담은 그 무엇인가를 뜻합니다. 즉, 물컵에 물이 담겨져 있으면 물은 `콘텐츠`고 물 컵은 `컨텍스트`라고 볼 수 있습니다.


