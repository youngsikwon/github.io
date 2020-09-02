---
layout: post
title: Swift 개발 디자인 패턴[SingleTon Pattern]
tags: [Swift]
comments: true
---

# Swift MVC

---

# Design pattern : What are patterns and why they understand

소프트웨어 설계 패턴은 애플리케이션의 구조를 설계할 때 직면할 수 있는 특정 문제에 대한 해결책을 제공합니다. 그러나 기본으로 제공하는 서비스나 오픈소스 라이브러리와 달리 설계 패턴은 코드가 아니기 때문에 응용 프로그램에 붙여 넣기만 할 수는 없습니다. 오히려, 그것은 문제를 해결하는 방법에 대한 일반적인 개념입니다. 디자인 패턴은 코드를 작성하는 방법을 알려 주는 템플릿이지만 이 템플릿에 코드를 맞추는 것은 사용자에게 달려 있습니다.


디자인 패턴은 다음과 같은 몇가지 이점을 제공합니다.

- 테스트된 솔루션: 디자인 페턴은 이미 최고의 솔루션을 제공하고 구현 방법을 알려 주므로 특정 소프트웨어 개발문제를 해결하기 위해 시간을 낭비하고 노력할 필요가 없습니다.

- 코드 통일: 디자인 패턴은 애플리케이션 아키텍쳐를 설계할 때 실수를 덜 하도록 결점과 버그에 대한 테스트를 거친 일반적인 솔루션을 제공합니다.

- 공통 어휘 : 소프트웨어 개발 문제를 해결하는 방법에 대한 자세한 설명을 제공하는 대신, 사용한 디자인 패턴을 다른 개발자가 즉시 이해 할 수 있도록 간단하게 말할 수 있다.


---

## iOS Swift에서 가장 흔히 접할 수 있는 디자인 패턴은 다음과 같습니다.

- 생성 패턴 : Singleton

## 1. Singleton 생성 패턴

싱글톤 패턴이란 특정용도로 객체를 하나 생성하여 공용으로 사용하고 싶을 때 사용하는 방법이다. 주로 환경설정, 로그인 정보 등을 특정용도로 생성해둔 객체에 넣어두고 여러객체에서 접근 가능하도록 하여 데이터를 사용하는 것이다. 이 객체는 임의로 메모리에서 해제 해주지 않는 이상 프로그램이 실행되고 끝날 때 까지 메모리에 유지된다.

IOS 개발에서 주로 사용하는 싱글톤 패턴의 객체는 다음과 같은 것들이 있다.

```swift
let screen = UIScreen.main
let userDefaults = UserDefaults.standard
let application = UIApplication.shared
let fileManager = FIleManager.default
let notification = NotificationCenter.default
```


- 싱글톤을 사용하지 않는 경우

스위프트를 이용한 iOS개발에서 클래스를 사용하여 생성한 객체들은 레퍼런스타입으로 참조에 의한 값의 전달이 이루어지지만, 객체를 새로 생성하는 경우에는 각각 개별적인 객체이며 소유하고 있는 프로퍼티의 값 또한 각자 다르다.

```swift
class NormalClass{
  var x = 0
}

let someObject1 = NormalClass()
someObject1.x = 5

let someObject2 = NormalClass()
someObject2.x = 1

let someObject3 = NormalClass()
someObject3.x = 10


someObject1.x
someObject2.x
someObject3.x
```

위 코드를 보면 someObject1, someObject2, someObject3은 각각 새로 생성한 NormalClass타입의 객체로 각각 다른 인스턴스이다. 따라서 그 프로퍼티인 x값도 각자 다르게 5, 1, 10으로 출력된다.



- 싱글톤을 사용한 경우

초반에 말했듯이 싱글톤패턴은 특정 객체가 앱에서 유일하게 하나만 존재하여 다른 객체들이 그 안의 내용을 공유하는 방식이다.

```swift
class SingletonClass{
  static let shared = SingletonClass()
  var x = 0
}

```
클래스를 정의할 때 내부에 해당 클래스와 같은 타입의 타입 프로퍼티를 생성하여 객체를 생성하지 않아도 접근이 가능하도록 한다. 이때 static 전역 변수로 선언하는데 이 프로퍼티는 지연생성(lazy)가 되기 때문에 처음 SingletonClass를 생성하기 전까지는 메모리에 올라가지 않는다.

```swift
let singleton1 = SingletonClass.shared
//여기서 SingletonClass.shared 하는 순간 static 프로퍼티가 생성되어 메모리에 올라간다.
singleton1.x = 10

let singleton2 = SingletonClass.shared // 두번째로 SingletonClass.shared하면 이미 위에서 SingletonClass가 생성되었기 때문에 그 인스턴스의 창조값을 전달합니다.
singleton2.x = 20
```
싱글톤 객체는 여러번 생성되는 것을 의도한 것이 아니라면 그것을 막아서 새로운 객체가 생성되어 유일성이 사라지는 문제를 해결해야 한다. 그 방법으로 해당 클래스 이니셜라이저를 private으로 설정하여 외부에서 또 다른 인스턴스을 생성하는 것을 막을 수 있다.
---

```swift
class MySingleton{
  static let shared =  MySingleton()
  var x = 0

  private init(){//생성자를 private으로 제한해서 외부에서 인스턴스를 생성하지 못하도록 강제하여, 유니크한 싱글톤을 만든다.
  }
}

이렇게 되면 하나의 싱글톤 인스턴스가 유지되어 여러 객체가 하나의 인스턴스에 접근하여 데이터를 공유할 수 있다.

```swift
let uniqueSingleton1 = PrivateSingleton.shared
let uniqueSingleton2 = PrivateSingleton.shared

uniqueSingleton1.x
uniqueSingleton2.x

uniqueSingleton1.x = 20

uniqueSingleton1.x
uniqueSingleton2.x

```
위의 코드를 보면 uniqueSingleton1.x의 값을 변경하면 uniqueSingleton2.x로 접근해도 같은 값을 얻을 수 있는 것을 알 수 있다.


예제) 객체를 하나만 생성하여, 생성된 객체를 어디서든 참조할 수 있도록 하는 패턴이며, Thread-safe 하게 작성하여 멀티스레드에서 사용해도 문제가 없어야 한다.


Java와는 다르게 Swift에서는 간결하게 구현이 가능한 듯 하다.

```swift
class Printer {
    static let shared = Printer()
    private init() {}

    func printDoc(_ doc: String) {
        print(doc)
    }
}
// static 변수에 Singleton 패턴을 적용할 크래스의 인스턴스를 할당해주면 된다.
```

- 클라이언트

```swift
Printer.shared.printDoc("Print Document")
//클라어
```


