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


### 
