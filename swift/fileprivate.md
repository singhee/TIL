# 접근한정자 - Access Control

`fileprivate`는 Swift 3.0 부터 추가된 새로운 접근 한정자이다. Swift 3.0에서 볼 수 있는 접근한정자에 대해서 간단하게 알아보면 다음과 같다.

* open: 모듈 외부에서 접근할 수 있는 가장 느슨한 접근한정자
* public: 모듈 외부에서 접근할 수 있지만, 상속은 되지 않고 `override`할 수 없다.
* internal: 모듈일 경우 접근이 가능하고 아무것도 쓰지 않는 경우 기본 설정되는 접근한정자이다.
* fileprivate: 파일일 경우 접근할 수 있는 접근한정자이다. 
* private: 클래스 등이 선언된 영역내에서만 접근이 가능하다.

위에서 open 과 fileprivate가 Swift 3.0 에서 추가된 새로운 접근 한정자이다. 그 중 fileprivate 에 대해서 알아보도록 하자.

## fileprivate

### Swift 2.x
```swift 
//Animal.swift
class BostonTerrier {
 private let name = "Boston Terrier"
}
 
class Dog {
   func callBostonTerrir() {
      let bt = BostonTerrier()
      print(bt.name)
   }
}
```

### Swift 3.0
```swift 
// Animal.swift
class BostonTerrier {
	fileprivate let name = "Boston Terrier"
}

class Dog {
	func callBostonTerrier {
		let bt = BostonTerrier()
		print(bt.name)
	}
}
```
Swift 2.x 까지는 같은 파일내에서 선언된 경우, private도 접근할 수 있었지만, Swift 3.0 부터는 명시적으로 fileprivate를 선언해야 접근할 수 있게 되었다. 이는 좀더 접근한정자를 세분화하여 명확하게 접근범위를 정했다는 것을 의미한다.