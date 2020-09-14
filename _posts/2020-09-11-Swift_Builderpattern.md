---
layout: post
title: Swift Builder Pattern
tags: [Swift]
comments: true
---


> Builder Pattern : 복잡한 객체의 생성을 그 객체의 표현과 분리하여, 생성 절차는 항상 동일하되 결과는 다르게 만드는 패턴.

---

&nbsp;UIKit을 사용해 iOS개발을 진행 하다보면 빠른 개발 속도를 위해 Storyboard를 사용 하지 않고 SwiftUI 코드만으로 UI 구성하게 되는 경우가 있다.
<br>
UI를 코드로 반복해서 구현 하다 보면, UI Component 마다 자주 호출되는 Property가 따로 있다는 것을 알게 된다.<br> 
예를 들어 UILabel을 구현할 떈 경험상 text, font, textColor, TextAlignment 순으로 자주 호출 한다.
<br>
UILabel 객체를 Builder 패턴을 사용해 생성하는 예제를 통해 자주 사용되는 Property를 어떻게 간단히 초기화 할 수 있는지 알아보자.

---
```swift
protocol BuilderType{
 associatedtype Product

    func build() -> Product
 }

 //Builder의 Protocol은 간단하다, Builder 객체가 생성할 클래스를 Product와, 이를 생성하는 메서드인 Builder()를 갖고 있다.
```


```swift
extension UILabel{
 typealias Builder = ULabelBuilder
  
}
class UILabelBuilder: BuilderType{
    private var frame: CGRect = .zero
    private var text: String? = nil
    private var font: UIFont? = nil
    private var textColor: UIColor? = nil
    private var textAlignment: NSTextAlignment = .left



    func withFrame(_ frame: CGRect) -> UILabelBuilder {
        self.frame = frame
        return self
    }

    func widthText(_ Text: String?) -> UILabelBuilder{
        self.text = text
        return self
    }

    func widthFont(_ font: UIFont?) -> UILabelBuilder{
        self.font = font
        return self
    }

    func widthTextColor(_ textColor: UIColor?) -> UILabelBuilder{
        self.textColor = textColor
        return self
    }
    
    func widthTextAlignment(_ textAlignment: NSTextAlignment) -> UILabelBuilder{
        self.textAlignment
        return self
    }



    func build() -> UILabel{
        let label: UILabel = .init(frame: self.frame)
        label.text = self.text
        label.font = self.font
        label.textColor = self.textColor
        label.textAlignment = self.textAlignment
        return label
    }
}
```

&nbsp;UILabel의 extension에 typealias를 사용해  UILabelBuilder가 UILabel의 내부 클래스 Builder인 것 처럼 사용할 수 있게 해주었습니다.<br>
UILabelBuilder의 Property들은 builder() 메서드 호출 시에 UILabel를 만들고 초기화하는데 사용 된다. 각각의 Property는 widthProperty() 메소드를 사용해 값을 넣어줄 수 있다. <br>
이 때 set이 아니라 width를 사용한 이유는 값을 변경한 이후 자기 자신을 반환해 메소드 체이닝이 가능하다는 것을 명시적으로 표현해주기 위함이다. 일반적으로 setter는 값을 반환 하지 않기 때문이다.
<br>
ex) 흰색 글씨, 시스템 폰트에 크기가 14, 가운데 정렬된 Hello world를 표시하는 UILabel를 Builder 패턴을 사용 하지 않은 방법과 Builder 패턴을 적용한 방법으로 각각 살펴 보기.

```swift
//절차적 구현
let label: UILabel = .init(frame: .zero)
label.text = "Hello world"
label.font = .systemFont(ofSize: 14)
label.textColor = .white
label.textAlignment = .center


//클로저를 사용한 구현
let label: UILabel = {
    let label: UILabel = .init(frame: .zero)
    label.text = "hello"
    label.font = .systemFont(ofSize: 14)
    label.textColor = .white
    label.textAlignment= .center
    return label
}()

// 빌더 패턴을 사용한 구현

let label: UILabel = UILabel.Builder()

.widthText("Hello world")
.widthFont(.systemFont(ofSize: 14))
.widthTextColor(.white)
.widthTextAlignment(.center)
.builder()
```

Builder pattern을 사용한 코드의 길아기 크게 줄지 않아 실망했을 수 있다. 그러나 실제 코드를 구현해보면 Label의 중복이 빠진 것만으로도 구현  속도의 차이가 나는 것을 경험 할 수 있다. 또한 label의 중복이 빠져서 생성 및 초기화 코드와 이후에 등장할 로직에 관련된 코드를 명확하게 분리할 수 있습니다.
<br>
클로저를 사용한 구현이 이러한 코드의 분리를 위해 사용되고는 하는데 Builder pattern을 사용하게 되면 클로저를 사용하는 것보다 코드의 길이는 더 짧아지면서 의미는 명확해지는 것을 확인할 수 있다.

