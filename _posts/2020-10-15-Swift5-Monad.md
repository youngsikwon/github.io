---
layout: post
title: Swift - 상속
tags: [Swift]
comments: true
---

# 상속(Inheritance)

---

클래스는 메소드, 프로퍼티와 다른 특징(characteristics)을 다른 클래스로부터 상속할 수 있습니다. 이것이 Swift에서 클래스가 다른 타입과 구분되는 근본적인 요소입니다. 클래스에서는 저장된 프로퍼티와 계산된 프로퍼티와 상관없이 상속받은 프로퍼티에 프로퍼티 옵저버를 설정해서 값 설정해서 반응 할 수 있습니다.



# 기반 클래스 정의(Defining a Base Class)

- 다른 어떤 클래스로부터 상속받지 않은 클래스를 기반 클래스라 합니다.

> NOTE
> Objective-C에서는 모든 클래스가 NSObject 클래스로부터 파생된 클래스로 생성되는 것과 달리 Swift에서는 SuperClass 지정 없이 클래스 선언이 가능하고 그 클래스가 SuperClass가 됩니다.


- 기반 클래스 예시

```swift
class Person{

  var name: String = ""
  var age: Int = 0

  var introduction: String {
    return "이름 : \(name), 나이 : \(age)"
  }


  func speck(){
print("가 나 다 라 마 바 사")
  }

}



let youngsik: Person = Person()

youngsik.name = "youngsik"
youngsik.age = 28

print(youngsik.introduction) // 이름 : 나이 출력

youngsik.speck() // 가나다라마사밧
```

- 예시 2

```swift
class Vehicle{ //탈 것.


  var currentSPeed = 0.0 //의 프로퍼티를 갖고 있고 
  var description: String { // description을 계산된 프로퍼티로 선언

    return "traveling at \(currentSPeed) miles per hour"
 
  }
  func makeNoise(){ // 이 메소드를 갖고 이 클래스를 이용해 탈 것 객체를 하나 생성합니다.

  let someVehicle = Vehicle()
  

  print("Vehicle: \(someVehicle.descirption)")
  }
}
```




# 서브클래싱(Subclassing)

- 상속, 다시 말해 서브클래싱을 하면 부모로부터 성격을 상속받고 자기 자신 고유의 특성도 추가할 수 있다.

```Swift

class SomeSubclass: SomeSuperclass{
  // subclass definition goes here 
}
```

위 탈것을 서브클래싱에 자전거라는 클래스를 선언하면 코드는 다음과 같습니다.

```swift

class Bicycle: Vehicle{
  var hasBasket = false
}

```

`Bicycle` 객체를 생성해 고유 속성을 사용할 수 있는 것을 확인할 수 있다.

```swift
bicycle.currentSpeed = 15.0
print("Bicycle: \(bicycle.description)")
// Bicycle: traveling at 15.0 miles per hour
```

물론 부모로부터 물렵다은 `currentSpeed`, `description`속성도 사용 가능합니다. 서브클래스로 생성된 클래스를 다시 서브클래싱 하는 것도 가능합니다.

```swift
class Tandem: Bicycle{
  var currentNumberOfPassengers = 0
}
```

`Bicycle`을 서브클래싱해서 `Tandem` 이라는 클래스를 생성하고 속성도 추가합니다.

```swift
let tandem = tandem()

tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
print("Tandem : \(tandem.description)")
// Tandem: traveling at 22.0 miles per hour
```

위 코드에서 `tendem`객체에서는 `tandem` 객체 고유 속성인 `currentNumberOfPassengers` 와 자신의 부모 `Bicycle`의 속성 그리고 조부모인 `Vehicle` 속성도 모두 사용 가능한 것을 확인할 수 있습니다.

---



