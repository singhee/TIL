# 함수 (Function)
스위프트에서의 함수는 일급 객체이기 때문에 하나의 값으로 사용할 수 있다. 또한 다른 언어보다 훨씬 다양한 모습으로 존재하며, 코딩 스타일도 여러가지이다. 따라서, 개인이나 협업자끼리 코딩 규칙을 만들고 함수를 사용하는게 좋다. 

## 함수와 메소드
함수와 메소드는 기본적으로 같다. 다만, 상황이나 위치에 따라 다른 용어로 부르는 것 뿐이다. 구조체, 클래스, 열거형 등 특정 타입에 연관되어 사용하는 함수를 **메소드**, 모듈 전체에서 전역적으로 사용할 수 있는 함수는 그냥 **함수**라고 부른다.

## 함수의 정의와 호출
함수와 메소드는 정의하는 키워드와 구현하는 방법은 같다. 조건문이나 반복문 같은 스위프트의 다른 문법과 달리 함수에서는 소괄호`()`를 생략할 수 없다. 스위프트의 함수는 재정의(Override)와 중복 정의(Overload)를 모두 지원한다. 그렇기 때문에 매개변수의 타입이 다르면 같은 이름의 함수를 여러 개 만들 수 있고, 매개변수의 개수가 달라도 같은 이름의 함수를 만들 수 있다. 

### 기본적인 함수의 정의와 호출
스위프트는 기본적으로 함수의 이름과 매개변수(Parameter), 반환타입(Return Type) 등을 사용하여 함수를 정의한다. 함수를 정의하는 키워드는 `func`이다.
```swift
func 함수 이름(매개변수...) -> 반환 타입 { // 매개변수는 소괄호`()`로 감싸주고, `->` 를 사용하여 어떤 타입이 반환될 것인지 명시해준다.
	실행구문
	return 반환 값
}
``` 

> **매개변수와 전달인자**
> 매개변수는 함수를 정의할 때 외부로부터 받아들이는 전달 값의 이름을 의미한다. 전달인자(Argument), 혹은 인자는 함수를 실제로 호출할 때 전달하는 값을 의미한다. 

### 매개변수
매개변수에 따라 함수의 모양과 기능이 어떻게 달라지는지 알아보자. 

#### 매개변수가 없는 함수와 매개변수가 여러 개인 함수
```swift
func helloWorld() -> String {
	return "Hello, World!"
}

print(helloWorld())		// Hello, World!
```
매개변수가 여러개 필요한 함수를 정의할 때는 `,`로 매개변수를 구분한다. 주의할 점은 함수를 호출할 때, 매개변수 이름을 붙여주고 `:`을 붙여 전달인자를 보내줘야 한다. 이렇게 호출 시에 매개변수에 붙이는 이름을 **매개변수 이름(Parameter Name)**이라 한다. 
```swift
func sayHello(myName: String, yourName: String) -> String {
	return "Hello \(yourName)! I'm \(myName)"
}

print(sayHello(myName: "sanghee", yourName: "you"))
```

### 데이터 타입으로서의 함수
스위프트의 함수는 **일급 객체**이므로 하나의 데이터 타입으로 사용될 수 있다. 각 함수는 매개변수 타입과 갯수, 반환 타입으로 구성된 하나의 타입으로 정의될 수 있다. 함수를 하나의 데이터 타입으로 나타내는 방법은 아래와 같다.
```swift
// (매개변수 타입의 나열) -> 반환 타입
```

```swift
func sayHello(name: String, times: Int) -> String {
	// ...
}
```
위의 sayHello 함수는 함수 타입이 `(String, Int) -> String`이라고 표현된다. 

```swift
func sayHelloToFriends(me: String, names: String...) -> String {
	// ...
}
```
위의 sayHelloToFriends 함수는 함수 타입이 `(String, String...) -> String` 이라고 표현된다. 만약 매개변수나 반환 값이 없다면 `Void` 를 사용하여 없음을 나타낸다. 

> 아래의 표현은 모두 매개변수나 반환 값이 없는 함수 타입을 나타낸다.	
> - `(Void) -> Void`
> - `() -> Void`
> - `() -> ()`

함수를 데이터 타입으로 사용할 수 있다는 것은 ++함수를 전달인자로 받을 수도, 반환 값으로 돌려줄 수도 있다++는 의미이다. 이는 스위프트의 함수가 일급 객체이기 때문에 가능한 일이다. 아래는 함수 타입의 사용과 관련된 예제 코드이다.

```swift
typealias CaculateTowInts = (Int, Int) -> Int

func addTowInts(_ a: Int, _ b: Int) -> Int {
	return a + b
}

func multiplyTowInts(_ a: Int, _ b: Int) -> Int{
	return a * b
}

var mathFunction: CalculateTwoInts = addTwoInts
// var mathFunction: (Int, Int) -> Int = addTwoInts 와 동일한 표현

print(mathFunction(2,5))	// 7

mathFunction = multiplyTwoInts

print(mathFunction(2,5))	// 10
```

스위프트의 함수는 또한 전달인자로 넘겨줄 수도 있다. 
```swift
func printMathResult(_ mathFunction: CalculateTwoInts, _ a: Int, _b: Int) {
	print("Result: \(mathFunction(a,b))")
}

printMathResult(addTowInts, 3, 5)	// Result: 8
```

이는 특정 조건에 따라 적절한 함수를 반환해주는 함수를 만들 수도 있다. 
```swift
func chooseMathFunction(_ toAdd: Bool) -> CalculateTwoInts {
	return toAdd? addTwoInts : multiplyTwoInts
}

printMathResult(chooseMathFunction(true), 3, 5)	// Result: 8
```


## 중첩 함수 
스위프트는 데이터 타입의 중첩이 자유롭다. 예를 들어 열거형 안에 또 하나의 열거형이 들어갈 수 있고, 클래스 안에 또 다른 클래스가 들어올 수 있다. 
함수의 중첩은 함수 안에 함수를 넣을 수 있다는 의미인데 함수 안의 함수로 구현된 중첩 함수는 상위 함수의 몸통 블록 내부에서만 사용할 수 있다. 물론, 함수가 하나의 반환 값으로 사용되면 상위 함수 외부에서 사용할 수는 있다.
```swift
func functionForMove(_ shouldGoLeft: Bool) -> MoveFunc {
	func goRight(_ currentPosition: Int) -> Int {
		return currentPosition + 1
	}

	func goLeft(_ currentPosition: Int) -> Int {
		return currentPosition - 1
	}
	return shouldGoLeft ? goLeft : goRight
}

var position: Int = -4	
let moveToZero: MoveFunc = functionForMove(position > 0)

while position != 0 {
	print("\(position)...")
	position = moveToZero(position)
}
```