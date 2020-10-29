---
layout: post
title: iOS Swift - RxSwift 사용 하면 좋은 이유?
tags: [Swift]
comments: true
---



# RxSwift는 무엇일까요?

> Swift에서 반응형 프로그래밍을 하기 위한 라이브러리입니다.

- RxSwift의 핵심 요소는 (Observable, Data Flow) 2가지 있다.


### Observable란?

반응한다는 것을 구현하기 위해서 Swift에서도 Observer Patten을 사용합니다. 마찬가지로 RxSwift에서도 Observer와 유사한 Observable을 사용하고 있습니다.
- Observable은 HOT과 COLD 2가지 방식을 나뉩니다.

![img5](../img/Observable.jpg)


### HOT Observable란?

HOT Observable은 이벤트가 발생하면 그걸 구독하고 있는 Subscriber가 그 이벤트를 받을 수 있습니다.<br>
만약 Subscriber가 없을 경우 Observable의 이벤트가 발생하면 그래도 이벤트를 계속 발송하려고 합니다.
(예시 - Facebook Live, Youtube Live) 생방송 같은 경우 시청자가 없어도 계속 송출한다.
Eager Evaluation: 조급한 개선법

### COLD Observable란?

COLD Observable은 반면에 Subscriber가 없으면 이벤트를 전달하지 않습니다.
(예시 - VOD 다시보기 서비스) 보는 사람이 보고 싶지 않으면 송출하지 않는다.
Lazy Evaluation: 느긋한 개선법


---

# 왜 사용하면 좋을까?

1. 반응형 패러다임이 제공하는 명확함
 - 비동기를 동기화 된 것인양 작성할 수 있다.

2. 일관성이 없는 비동기 코드 해결
 - 어떤 곳에서는 DispatchQueue, 어떤 곳에서는 OperationQueue...
 - 하나의 비동기 코드로 개발이 가능.

3. 확장 불가능한 아키택처 패턴을 해결
 - 일관성 없는 비동기 코드를 작성하게 되어 서로 다르게 구현한 로직을 조합하거나 확장하기에 어려운 부분을 해결
 - Rx로 일관된 코드를 작성하면서 아키택처의 확장이 가능


4. 콜백지옥에서 벗어나기 좋음
 - UI이벤트, 네트워크 처리 및 데이터 갱신 등에서 콜백 지옥이나 이벤트 등록 후 추적, 스레드를 넘나들면서 작업 했던 경험이 있으시다면, 이제 RxSwift를 이용하여 가독성을 높이고 스레드를 쉽게 넘나들며 콜백 지옥을 탈출 할 수 있음.
 - [참고](https://www.youtube.com/watch?v=jCT-eUaD-d4) - Youtube 영상

5. Rx에겐 뭔가 특별한 것이 있다.
 - 위에서 언급한 라이브러리들과 용도나 동작은 같으나, Rx에는 뭔가 특별한 것이 있다. 그것이 바로 `Operator` 입니다.
 - `Async`하게 전달되는 데이터를 조작해서 사용하는 부분이 다른 비동기 유틸과 차별되는 부분이죠. 그리고 그 기능이 굉장히 다양하고 강력해서 이 부분을 어렵다고 느끼는 분들이 대다수입니다. 사실 이 부분을 대충 어떠한것들이 있는지 알고 나중에 필요할 떄가 되면 적절한 것을 찾아 낼 수 있으면 됩니다. 그래서 [ A Decision Tree of Observable Operators](http://reactivex.io/documentation/operators.html) 에서 가이드를 제공하고 있으니 참고 하시면 됩니다.
 - 이렇게 놓고 보면 Rx는 전혀 어려울 것이 없다. 학습 곡선이 높다는 주변의 말만 듣고 배우고 사용하는데 겁먹을 필요가 없습니다. 그러니 필요하면 쓰세요.
 - [참고한 링크](https://iamchiwon.github.io/page/8/)


6. TDD에 적응하기 좋다.
 - TDD의 제대로 적용 해본적이 없습니다...(TDD 앞으로 공부할 예정) 백엔드 등에서 TDD 도입할 때도 좋다고 합니다.

7. 코드가 깔끔해진다.
 - 모던하게 코드가 깔끔해지고 편하다
 - 가독성이 좋습니다

8. Thread 처리가 참 쉽습니다.
 - MainThread에서 처리하기, 백그라운드에서 처리하다 다시 메인 다시 백그라운드.. 쉽게 처리할 수 있습니다.
 - 제대로 멀티 스레드 코드를 작성 하면 많은 비용(구현이 많이 어려워요)이 들어요.. 그것을 낮춰줍니다.


9. 트랜드 개발의 만족감
 - 모든 개발자는 그렇지 않겠지만, 제 생각에 계속 개발 학습하는걸 즐깁니다. Rx도 새로운 도전이고 학습할 거리입니다. 개발 신입이 objective-C로 된 회사 소스를 배우고 시작하는 것보다 트랜디한 개발을 학습할 수 있는 것도 장점이라고 생각합니다.

 10. 이직할 때 약팔기 좋음
  - 현재 많은 회사들이 Rx를 도입해서 개발하고 있습니다. 그러면? 러닝커브 없거나 적은 개발자를 원하겟죠?

11. 자바, 코틀린, 자바스크립트, 다른 모든 진영의 Rx개발자와 대화가 가능

 - 같은 Rx 기능을 사용하기 때문에 다른 언어이지만 같은 대화가 가능합니다. 어떤 오퍼레이터로 해서 해결 가능하다? 어떤 것을 사용하세요. 전 이렇게 해결 했는데..

12. 지원(서포트, 제공)하는 Operator들이 정말 많음.
 - [지원 서포터 자료](http://reactivex.io/documentation/ko/operators.html)



 ## 문제점은 무엇일까요?

 1. 러닝 커브
  - 학습하기가 어렵긴합니다. 하지만 점점 좋은 자료들이 많이 많이 생기고 있다.
   - [러닝 커브 블로그](https://github.com/ClintJang/awesome-swift-korean-lecture/blob/master/README.md#rxswift)
2. Debugging이 어렵다
 - 스텍이 깊게 있어서 트래킹하기 어려운 느낌입니다.
 - 그래도 할만합니다. 제공하는 Operator를 활용할 수도 있습니다.
 - [Debug Operator](https://github.com/ReactiveX/RxSwift/blob/main/RxSwift/Observables/Debug.swift)를 이용하면 많은 해결이 가능하다.

3. 클로저의 캡처리스트(closure capture list)를 신경써야됩니다.
 - 클로저 사용이 많습니다. 캡처 리스트를 사용하면 메모리 누수를 일으키는 강한 순환 참조(strong reference cycle)를 피할 수 잇게 신경서야됩니다.
 - 클로저에서는 value type이라고 하더라도, 해당 객체가 만들어진 곳의 인스턴스를 참조할 것입니다. 캡처 리스트를 해주어야 됩니다. 안그럼 race condition 같은것이 발생할 수 있습니다.
 - 그런데 이것은 `swift Closure`와 관련 된 부분이죠.. 다른 라이브러리도..
 - [클로져와 메모리 해제 실험](https://github.com/iamchiwon/Closure_ARC_Experiment)
 - [Do you often forget [weak self]? Here’s a solution 링크](https://medium.com/anysuggestion/preventing-memory-leaks-with-swift-compile-time-safety-49b845df4dc6)

4. Swift에서 Async/await가 나온다면..
 - Swift 5.0에는 반영이 안된다고 하는 것 같습니다. 추후에는 반영될 것 같아요.. 그때는 달라질 지도 모르겠네요. 올해는 아닌 것 같습니다.
 - Rx를 Async/await로 갈아타기 보다는 RxSwift 내부 구조가 많이 바뀌는 식이 될 수 있다고 생각함.


 