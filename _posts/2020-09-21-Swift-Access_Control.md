---
layout: post
title: Swift5 - 접근제어란?
tags: [Swift]
comments: true
---



> 접근제어(Access control)
> 코드끼리 상호작용을 할 때 파일 간 또는 모듈 간에 접근을 제한할 수 잇는 기능입니다. 접근제어를 통해 코드의 상세 구현은 숨기고 허용된 기능만 사용하는 인터페이스를 제공합니다.

객체지향 프로그래밍 패러다임엣 중요한 캡슐화와 은닉화를 구현하는 이유는 외부에서 보거나 접근하면 안 되는 코드가 있기 떄문입니다.<br>

불필요한 접근으로 의도치 않은 결과를 초래하거나 꼭 필요한 부분만 제공을 해야하는데 전체 코드가 노출될 가능성이 높을 때 접근제어를 사용합니다.

> 튜플의 접근수준 부여

```swift
internal class InternalClass {}
private struct PrivateStruct{}

// 요소로 사용되는 InternalClass와 PrivateStruct의 접근 수준이
// publicTuple보다 낮기 떄문에 사용 할 수 없습니다.

public var publicTuple(first: InternalClass, second: PrivateStruct)
 = (InternalClass(), PrivateStruct())


private var priateTuple(first: InternalClass, second: PrivateStruct)
 = (InternalClass(),PrivateStruct())
```

> 접근 수준에 따른 접근 결과


```swift
//AClass.swift 파일과  Common.swift 파일이 같은 모듈에 속해 있을 경우

//AClass.swift 파일

class AClass{
  func internalMethod(){}
  fileprvate func fileprivate() {}
  var internalProperty = 0
  fileprivate var fileProperty = 0
}


//Common.swift 파일

let aInstance: AClass = AClass()
aInstance.internalMethod ()// 같은 모듈이므로 호출 가능
aInstance.fileprivate() // 다른 파일이므로 호출 불가  - 오류
aInstance.internalProperty = 1 //같은 모듈이므로 접근 가능
aInstance.filePrivateProperty = 1 // 다른 파일이므로 접근 불가 - 오류
```

> 열거형의 접근 수준

```swift
private typealias PointValue = Int

// 오류 - PointValue가 Point보다 접근 수준이 낮기 떄문에 원시 값으로 사용할 수 없습니다.

enum Point: PointValue{
  case x, y
}
```

> 같은 파일에서의 private와 fileprivate

```swift
public struct SomeType{
  private var privateVariable = 0
  fileprivate var fileprivateVarible = 0
}


// 같은 타입의 익스텐션에서는 private 요소에 접근 가능
extension SomeType{
  public func publicMethod(){
    print("\(self.privateVariable). \(self.fileprivateVarible)")
  }

  private func privateMethod(){
   print("\(self.privateVariable). \(self.fileprivateVarible)")
  }

  fileprivate func fileprivateMethod(){
    print("\(self.privateVariable). \(self.fileprivateVarible)")
  }
  
  struct AnotherType {
    var someInstance: SomeType = SomeType()

    mutating func someMethod(){
      // public 접근수준에는 어디서든 접근 가능
      self.someInstance.publicMethod() // 0, 0

      // 같은 파일에 속해 있는 코드이므로 private 접근 수준 요소에 접근 가능
      self.someInstance.fileprivateVarible = 100
      self.someInstance.fileprivateMethod() // 0, 100


    }
  }
}
var anotherInstace = AnotherType()
anotherInstace.someMethod()
```

