# 클로저 (closure)
## 후행 클로저 (Trailling Closure)
함수나 메소드의 마지막 전달인자로 위치하는 클로저는 함수나 메소드의 소괄호를 닫은 후 작성해도 된다. Xcode에서 자동완성 기능을 사용하면 자동으로 후행 클로저를 유도한다. 단, 후행 클로저는 **맨 마지막 전달인자로 전달되는 클로저에마나 해당되므로** 전달인자로 클로저가 여러 개 전달될 때에는 맨 마지막 클로저만 후행 클로저로 사용할 수 있다. 또한, 단 하나의 클로저만 전달인자로 전달하는 메소드의 경우에는 소괄호를 생략해줄 수도 있다.

```swift
// 후행 클로저의 사용
let reversed: [String] = names.sorted() { (first: String, second: String) -> Bool in 
	return first > second 
}

// sorted(by:) 메서드의 소괄호까지 생략 가능하다.
let reversed: [String] = names.sorted { (first: String, second: String) -> Bool in
	return first > second
}
```
후행 클로저는 Xcode에서도 자동완성으로 후행 클로저를 사용하도록 유도하므로 자주 볼 수 있을 것이다.
