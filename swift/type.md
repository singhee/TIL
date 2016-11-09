# 타입
`Swift`타입은 세 가지 기본 그룹으로 구분되는데 **구조체, 클래스, 열거형** 이다. 이 세가지 그룹들은 모두 다음을 가질 수 있다.
	
	1. 프로퍼티 (properties) : 타입과 관련된 값들
	2. 이니셜라이저(initializers) : 타입의 인스턴스를 초기화하는 코드
	3. 인스턴스 메소드(instance methods) : 타입의 인스턴스에서 호출할 수 있는 해당 타입의 특정 함수
	4. 클래스 메소드 또는 정적 메소드(class or static methods) : 타입 자체에서 호출할 수 있는 해당 타입의 특정 함수

`Swift`의 구조체나 열거형은 프로퍼티, 이니셜라이저, 메소드 뿐만 아니라 확장하거나 프로토콜을 따를 수도 있다.
숫자나 boolean 값과 같이 일반적인 원시 타입은 `Swift`에서는 모두 구조체로 구현되어있다. 다음의 타입들은 모두 구조체이다.
	
>숫자 : Int, Float, Double
 불린 : Bool
 텍스트 : String, Character
 컬렉션 : Array<T>, Dictionary<K:Hashable, V>, Set<T:Hashable>

기본 타입들이 구조체로 되어있기 때문에 이는 프로퍼티, 이니셜라이저, 메소드를 가지고 있음을 의미한다. 또한 프로토콜을 따르거나 확장될 수도 있다.

## 기본 타입 사용하기
`var` 키워드는 변수를 표시한다. 따라서 `var`로 선언된 변수의 값은 초기 값에서 바뀔 수 있다. 
```swift
var str = "Hello, playground"	// Hello, playground
str = "Hello, Swift"			// Hello, Swift
```

`let` 키워드는 값을 바꿀 수 없는 상수 값을 표시한다. 자신의 코드에서 값을 바꿀 필요가 없으면 `let`을 사용해야 할 것이다.
```swift
let constStr = "Hello, Swift"	// Hello, Swift
constStr = "Hello, world"		// Error : Cannot assign to value : 'constStr' is a 'let' constant 
```

## 타입 추론
위의 코드에서 변수나 상수에 타입을 명시하지 않았음을 볼 수 있다. 이는 **타입이 없음을 뜻하지 않는다.** 대신 컴파일러는 초기 값으로 그 타입을 **추론**한다. 이를 **타입 추론(type inference)** 이라 한다.

## 타입 지정
상수나 변수가 초기 값을 가지면 타입 추론에 의존할 수 있다. 그러나 상수나 변수가 초기값을 가지고 있지 않거나 명확한 타입을 보장하고 싶다면 그 선언에 타입을 명시할 수 있다. 
```iOS
var nextYear : Int
var bodyTemp : Float
var hasPet : Bool
```

### 숫자와 불린 타입
정수의 가장 일반적인 타입은 `Int`다. 워드 크기와 부호 여부에 따라 추가적인 정수 타입이 있지만, 애플에서는 정말로 다른 것을 사용해야 할 이유가 있지 않는 한 `Int`의 사용을 권장한다.
`Swift`에서 부동 소수점 수는 정밀도에 따라 세 가지 타입을 제공한다. 32bit 수인`Float`, 64bit 수인`Double`, 80bit 수인`Float80`이다. 또한 불린값은 `Bool`타입을 사용하여 표현한다. `Bool`의 값은 `true`나 `false`이다.

### 컬렉션 타입
`Swift` 표준 라이브러리는 세 가지 컬렉션인 **배열(array), 딕셔너리(dictionary), 셋(set)**을 제공한다.
**배열(Array)**은 순서가 있는 요소들의 모임이다. 배열 타입은 `Array<T>`로 쓴다. 여기서 `T`는 배열에 포함될 요소의 타입으로 기본 타입, 구조체, 클래스 등 어떤 타입의 요소든 포함될 수 있다. 배열을 선언하는 축약형이 있는데, 단순히 포함될 요소의 타입을 꺽쇠괄호로 감싸는 것이다. 
```
var hasPet : Bool
var arrayOfInts: Array<Int>  // 일반적 선언 
var arrayOfInts: [Int]	 	 // 축약형 선언 
```

**딕셔너리(Dictionary)**는 순서가 없는 `key-value`쌍의 모음이다. 값은 구조체나 클래스를 포함하여 어떤 타입이든 될 수 있다. key 도 어떤 타입이든 될 수 있지만 반드시 고유해야 한다. 특히 key는 **해싱(hashing)**이 가능해야 한다. 해싱은 딕셔너리가 고유한 key를 가지고 주어진 key를 통해 값에 좀 더 효율적으로 접근할 수 있는것을 보장한다. Swift의 기본타입들은 해싱이 가능하다. 딕셔너리 선언에도 축약형이 있다.
```
var arrayOfInts : [Int]
var dictionaryOfCapitalsByCountry : Dictionary<String, String>  // 일반적 선언
var dictionaryOfCapitalsByCountry : [String:String] 		    // 축약형 선언
```

**셋(Set)**은 특정 타입의 요소들은 포함한다는 점에서 배열과 유사하지만, 순서를 갖지 않고 멤버 또한 해싱 가능해야 하며 고유해야 한다. 셋은 멤버인지 확인할 때 더 빠르게 만든다. 
```
var winningLotteryNumbers: Set<Int>
```

### 이니셜라이저 (Initializer)
**인스턴스(instance)**를 만드는 방법으로 해당하는 타입의 이니셜라이저(initializer)를 사용할 수 있다. 이니셜라이저는 타이브이 새 인스턴스 내용을 초기화하는 역할을 한다. 이니셜라이저로 새 인스턴스를 만들려면 타입명 다음에 괄호 쌍을 사용하고 필요한 경우에 인자를 더하면 된다.
일부 기본 타입은 아무론 인자가 없을 때 빈 리터럴을 반환하는 이니셜라이저를 가지고 있다.
```swift
let emptyString = String()				// ""
let emptyArrayOfInts = [Int]()			// []
let emptySetOfFloats = Set<Float>()		// []
```
이와 달리 다른 타입들은 기본 값을 가진다.
```swift
let defaultNumber = Int()	// 0
let defaultBool = Bool()	// false
```

### 프로퍼티 (Property)
프로퍼티는 타입의 인스턴스와 연관된 값이다. 예를들면 `String`은 해당 문자열이 비어있는지의 여부를 알려주는 `Bool`타입의 프로퍼티 `isEmpty`를 가진다. `Array<T>`는 배열의 요소 수를 알려주는 `Int`타입의 프로퍼티 `count`를 가진다.

### 인스턴스 메소드 (Instance Method)
인스턴스 메소드는 특정 타입에 정의하여 그 타입의 인스턴스에서 호출될 수 있는 함수이다. `Array<T>`에서 인스턴스 메소드 `append(_:)` 를 불러보자.
```swift
var countingUp = ["one", "two"]		// ["one", "two"]
countingUp.count  					// Array 프로퍼티

countingUp.append("three")			// Array 인스턴스 메소드
									// ["one", "two", "three"]
```


