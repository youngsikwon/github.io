---
layout: post
title: Swift 개발 디자인 패턴[Strategy Pattern]
tags: [Swift]
comments: true
---

# Swift Design pattern

---

# Design pattern : What are patterns and why they understand

소프트웨어 설계 패턴은 애플리케이션의 구조를 설계할 때 직면할 수 있는 특정 문제에 대한 해결책을 제공합니다. 그러나 기본으로 제공하는 서비스나 오픈소스 라이브러리와 달리 설계 패턴은 코드가 아니기 때문에 응용 프로그램에 붙여 넣기만 할 수는 없습니다. 오히려, 그것은 문제를 해결하는 방법에 대한 일반적인 개념입니다. 디자인 패턴은 코드를 작성하는 방법을 알려 주는 템플릿이지만 이 템플릿에 코드를 맞추는 것은 사용자에게 달려 있습니다.


디자인 패턴은 다음과 같은 몇가지 이점을 제공합니다.

- 테스트된 솔루션: 디자인 페턴은 이미 최고의 솔루션을 제공하고 구현 방법을 알려 주므로 특정 소프트웨어 개발문제를 해결하기 위해 시간을 낭비하고 노력할 필요가 없습니다.

- 코드 통일: 디자인 패턴은 애플리케이션 아키텍쳐를 설계할 때 실수를 덜 하도록 결점과 버그에 대한 테스트를 거친 일반적인 솔루션을 제공합니다.

- 공통 어휘 : 소프트웨어 개발 문제를 해결하는 방법에 대한 자세한 설명을 제공하는 대신, 사용한 디자인 패턴을 다른 개발자가 즉시 이해 할 수 있도록 간단하게 말할 수 있다.


---

## iOS Swift에서 가장 흔히 접할 수 있는 디자인 패턴은 다음과 같습니다.

- 스트래티지 패턴

## 스트래티지 패턴이란

> 행위를 클래스로 캡슐화해 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴
 - 같은 문제를 해결하는 여러 알고리즘이 클래스별로 캡슐화되어 있고 이들이 필요할 때 교체할 수 있도록 함으로써 동일한 문제를 다른 알고리즘으로 해결할 수 있게 하는 디자인 패턴
 - 즉, 전략을 쉽게 바꿀 수 있도록 해주는 디자인 패턴.
  - 전략이란 : 어떤 목적을 달성하기 위해 일을 수행하는 방식, 비즈니스 규칙, 문제를 해결하는 알고리즘.
 - 특히 게임 프로그래밍에서 게임 캐릭터가 자신이 처한 상황에 따라 공격이나 행동하는 방식을 바꾸고 싶을 때 스트래티지 패턴은 매우 유용하다.


 ## 예시

 > 로봇 프로토콜과 그것을 따르는 태권V, 아톰 객체 생성하는 예시를 통해 스트래티지 패턴을 학습해본다.



#### AS-IS

Robot 클래스

```swift
protocol Robot {
  var name: String {get}
  init(name: String)
  func attack()
  func move()
}

class TaekwonV: Robot{
  var name: String


  required init(name: String){
    self.name = name
  }


  func attack(){
    print("I launch Missle")
  
  
  }

  func move(){
    print("I can only Walk")
  }

}

class Atom: Robot {
  var name: String


  required init(name: String){
    self.name = name
  }


  func attack(){
    print("I have Punch")
  }
  func move(){
    print("I can Fly")
  }
}
```

Client

```swift
let taekwonV = TaekwonV(name: "TaekwonV")
print("My name is \(taekwonV.name)")    // My name is TaekwonV
taekwonV.attack()                       // I launch Missle
taekwonV.move()                         // I can only Walk

let atom = Atom(name: "Atom")
print("My name is \(atom.name)")        // My name is Atom
atom.attack()                           // I have Punch
atom.move()                             // I can Fly
```


문제점

로봇 클래스를 캡슐화 하여 생성할 수 있도록 하는 기본적인 코드이다. 현재로서는 큰 문제가 없지만 기존 클래스에 수정을 가하거나, 새로운 클래스를 추가하고자 할 때 당므과 같은 문제가 발생합니다.


- 기존 행위를 수정(ex. 펀치 공격의 코드를 수정하고 싶은 경우)
 - 펀치 공격을 사용하는 모든 클래스의 개별 코드를 수정해야 한다.
- 새로운 로봇 클래스를 추가(ex. 미사일 공격이 가능하고, 날 수 있는 로봇)
 - 태권V의 미사일 공격 코드와 아톰의 비행 코드가 중복된다.



 ## TO-BE

 이 문제를 해결 하기 위해 공통적인 행위를 캡슐화 하여 사용한다. 이 예시에서는 공격 행위와 이동 행위를 캡슐화 할 수 있다.


 Strategy 클래스
```swift
protocol AttackStrategy {
    func attack()
}

class MissleStrategy: AttackStrategy {
    func attack() {
        print("I launch Missle")
    }
}

class PunchStrategy: AttackStrategy {
    func attack() {
        print("I have Punch")
    }
}

protocol MoveStrategy {
    func move()
}

class FlyStrategy: MoveStrategy {
    func move() {
        print("I can Fly")
    }
}

class WalkStrategy: MoveStrategy {
    func move() {
        print("I can only Walk")
    }
}
```


공통된 행위를 Abstract 하여 작성해주었다. 개별 행위에 대한 수정 사항이 있을 경우 이 부분을 수정해주면 되고, 새로운 행위의 추가도 이 부분에서만 작업 해주면 된다.


Robot 클래스

```swift
protocol Robot {
    var name: String { get }
    var attackStrategy: AttackStrategy { get set }
    var moveStrategy: MoveStrategy { get set }
    init(name: String, attackStrategy: AttackStrategy, moveStrategy: MoveStrategy)
}

extension Robot {
    func attack() {
        self.attackStrategy.attack()
    }

    func move() {
        self.moveStrategy.move()
    }

    mutating func setAttack(attackStrategy: AttackStrategy) {
        self.attackStrategy = attackStrategy
    }

    mutating func setMove(moveStrategy: MoveStrategy) {
        self.moveStrategy = moveStrategy
    }

}

```

Protocol로 기본적인 Abstract 요소를 정의해주고, 기본 행위를 지정해주기 위해 Extension을 활용한다. 
Initializer 행위에 대한 Strategy의 기본값으 파라미터로 받아 객체를 생성하고, 별도의 Setter 메소드를 두어 행위의 반환이 자유롭도록 만들어주었다.

```swift
class TaekwonV: Robot {
    var name: String
    var attackStrategy: AttackStrategy
    var moveStrategy: MoveStrategy

    required init(
        name: String,
        attackStrategy: AttackStrategy = MissleStrategy(),
        moveStrategy: MoveStrategy = WalkStrategy()
    ) {
        self.name = name
        self.attackStrategy = attackStrategy
        self.moveStrategy = moveStrategy
    }
}

class Atom: Robot {
    var name: String
    var attackStrategy: AttackStrategy
    var moveStrategy: MoveStrategy

    required init(
        name: String,
        attackStrategy: AttackStrategy = PunchStrategy(),
        moveStrategy: MoveStrategy = FlyStrategy()
    ) {
        self.name = name
        self.attackStrategy = attackStrategy
        self.moveStrategy = moveStrategy
    }
}
```
개별 로봇을 생성할 때 인자로 넘겨주는 Strategy의 기본값에 차별을 둔다. 생성 이후에도 Setter 메소드를 통해 행위의 Strategy는 변경 가능하다.

```swift
let taekwonV = TaekwonV(name: "TaekwonV")
print("My name is \(taekwonV.name)")    // My name is TaekwonV
taekwonV.attack()                       // I launch Missle
taekwonV.move()                         // I can only Walk

let atom = Atom(name: "Atom")
print("My name is \(atom.name)")        // My name is Atom
atom.attack()                           // I have Punch
atom.move()                             // I can Fly
```
- 클라이언트의 코드와 실행 결과는 AS-IS와 동일하다






