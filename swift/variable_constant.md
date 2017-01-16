# 변수와 상수 
변수`variable`는 값의 수정이 가능하고, 상수`constant`는 그렇지 않다. Swift에서는 언제 어디서 값이 어떻게 바뀔지 모르는 변수보다 안전한 상수를 사용하는 것을 권장하고 있다. 

변수는 `var`, 상수는 `let`으로 선언한다.
```swift
var name = "sanghee Yoon"
let birthyear = 1994

name = "윤상희"
birthyear = 2017		// 컴파일 에러!
```

Swift는 정적 타이핑 언어이다. 이는 변수나 상수를 정의할 때 그 자료형 타입이 어떤 것인지를 명시해주어야 하는 언어를 말한다.
```swift
var name: String = "sanghee Yoon"
let birthyear: Int = 1994
var height: Float = 160.0
```

Swift에서는 또한 타입을 굉장히 엄격하게 다루기 때문에, 다른 자료형끼리는 기본적인 연산조차 되지 않는다. 아래와 같이 `Int` 타입인 `birthyear`와 `Float`타입인 `height`를 더하려고 하면 컴파일 에러가 발생한다.
```swift
birthyear + height // 컴파일 에러!
```

위와같은 에러는 일반적인 다른 프로그래밍 언어라면 상상하기 어려운 일이다. 이럴 때에는 Swift에서는 명확하게 다음과 같이 사용하여야 한다.
```swift
Float(birthyear) + height 
```

조금만 더 응용하면 아래와 같이 숫자를 문자열로 만들 수도 있게 된다.
```swift
String(birthyear) + "년생" + name + "입니다."
```

하지만 위와 같은 경우는 Swift에서 더 간단하게 작성할 수도 있다.
```swift
"\(birthyear)년생 \(name) 입니다."
```

## 배열(Array)와 딕셔너리(Dictionary)
배열과 딕셔너리는 모두 대괄호`[]`를 이용해서 정의할 수 있다.
```swift
var languages = ["Swift", "Objective-C", "Python"]
var capitals = [
  "한국": "서울",
  "일본": "도쿄",
  "중국": "베이징",
]
```

배열과 딕셔너리에 접근하거나 값을 변경할 때에도 대괄호를 사용한다.
```swift
languages[0] // Swift
languages[1] = "Ruby"

capitals["한국"] // 서울
capitals["프랑스"] = "파리"
```

배열과 딕셔너리는 다른 상수와 마찬가지로 `let`으로 정의하면 값을 수정할 수 없다. 배열과 딕셔너리의 타입 선언도 역시 대괄호 `[]`안에 어떤 타입을 받을 건지 명시하여 선언한다.
```swift
var languages: [String] = ["Swift", "Objective-C", "Python"]
var capitals: [String: String] = [
  "한국": "서울",
  "일본": "도쿄",
  "중국": "베이징",
]

var lanhuages: [String] = []			// 빈 배열 정의
var capitals: [String: String] = [:]	// 빈 딕셔너리 정의
```

위에서 빈 배열과 빈 딕셔너리로 선언하는 것을 조금 더 간결하게 하고 싶다면, 타입 뒤에 괄호 `()`를 쓰면된다. 괄호`()`를 쓰는 것은 생성자`initializer`를 호출하는 것이다.
```swift
var languages = [String]()
var capitals = [String: String]()
```

