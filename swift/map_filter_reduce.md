# Functional Programming in Swift  - Chapter 04 - Map, Filter, Reduce  (맵, 필터, 리듀스)
## Map (맵)
```swift
func incrementArray(xs: [Int]) -> [Int] {
	var result: [Int] = []
	for x in xs {
		result.append( x + 1 )
	}
	return result 
}
```

```swift 
func doubleArray(xs: [Int]) -> [Int] {
	var result: [Int] = []
	for x in xs {
		result.append( x * 2 )
	}
	return result 
}
```

위의 두 함수를 하나의 함수로 추상화 하여 나타내려면? 배열의 개별요소에서 새 정수를 계산할 함수를 추가적인 파라미터로 받아야 한다. 
```swift
func computeIntArray(xs: [Int], transform: (Int) -> Int) -> [Int] {
	var result: [Int] = []
	for x in xs {
		result.append(transform(x))	
	}
	return result
}
```

위의 새로운 함수를 통해 incrementArray와 doubleArray는 더 간단해진다.
```swift
func incrementArray(xs: [Int]) -> [Int] {
	return computeIntArray(xs) { x in x + 1 }
}

func doubleArray(xs: [Int]) -> [Int] {
	return computeIntArray(xs) { x in x * 2 }
}
```

만약, 원래 배열의 숫자가 짝수인지 아닌지를 나타내는 불리언 값을 담은 새 배열을 계산하고 싶다면???
```swift
func isEvenArray(xs:[Int]) -> [Bool] {
	return computeIntArray(xs) { x in x % 2 == 0 }
}
```
위의 코드는 타입 오류를 일으키게 된다.  computeIntArray가 [Int] 를 반환하기 때문이다.  
이 문제를 어떻게 해결해야 할까? computeIntArray 대신 아래와 같이 [Bool]을 반환하는 새로운 computeIntArray를 정의하는 것이다.
```swift
func computeBoolArray(xs: [Int], transform: (Int) -> Bool) -> [Bool] {
	var result: [Bool] = []
	for x in xs {
		result.append(transform(x))
	}
	return result
}
```

하지만 이는 반환하는 타입이 바뀔때마다 또 다른 고차함수를 만들어야 하는 번거로움이 있다 . 다행히도 이 문제를 해결할 방법이 있다. 바로 제네릭을 사용하는 것이다. 
computeIntArray와 computeBoolArray의 유일한 차이점은 타입 선언 뿐이다.  그러니 다음과 같이 가능한 모든 타입에 동작하는 하나의 제네릭 함수를 작성하는 것이 보다 나은 방법이다. 
```swift
func genericComputeArray1<T>(xs: [Int], transform: Int -> T) -> [T] {
	var result: [T] = []
	for x in xs {
		result.append(transform(x))
	}
	return result
}

// 밑에서의 일반화한 함수(map)를 적용시켰을 때 
func genericComputeArray2<T>(xs: [Int], transform: Int -> T) -> [T] {
	return map(xs, transform: transform)
}
```

[Int] 타입의 입력 배열에만 작동해야할 필요가 없기 때문에, 이 부분도 추상화 한다면 아래와 같이 함수를 더 일반화 할 수 있다.  
```swift
func map<Element, T>(xs: [Element], transform: Element -> T) -> [T] {
	var result: [T] = []
	for x in xs {
		result.append(transform(x))
	}
	return result
}
```

::genericComputeArray 함수는 위의 map 함수의 좀 더 구체적인 타입을 갖는 인스턴스 이다.  아래 처럼  최상위 map 함수를 정의하는 대신, Array의 확장으로 map을 정의하는 것이 스위프트의 관례에 더 잘 맞는다.:: 
```swift
extension Array {
	func map<T>(transform: Element -> T) -> [T] {	// 여기서의 Element 타입은 Element에 대해 제네릭인 스위프트의 Array의 정의로부터 나온다. 
		var result: [T] = []
		for x in self {
			result.append(transform(x))
		}
		return result
	}
}
```

Array의 확장으로 map을 정의함으로써 , `map(xs, transform)`대신 Array의 map 함수를 사용해  `xs.map(transform)` 이라고 작성할 수 있다.! WOW 
(실제로 Swift  표준 라이브러리에 유사하게 정의 되어있어서 손쉽게 Array에 map함수를 적용할 수 있다.  - SequenceType Protocol 에 정의되어 있다. ) 
```swift
// 최상의 map 함수를 직접 정의한 경우
func genericComputeArray2<T>(xs: [Int], transform: Int -> T) -> [T] {
	return map(xs, transform: transform)
}

// Array의 extension으로 map을 정의한 경우
func genericComputeArray3<T>(xs: [Int], transform: Int -> T) -> [T] {
	return xs.map(transform)
}
```


### 최상위 함수 vs 확장(Extension)

map 함수를 정의하는 두 가지 방법에 대해서 보았다. 표준 라이브러리의 첫 버전에서는 대부분 최상위 함수를 사용한 시도가 많았지만, 스위프트 2.0 부터는 `Protocol Extension`의 등장으로 써드 파티 개발자들도 Array와 같은 특정 타입과 더불어 SequenceType과 같은 프로토콜에도 자신만의 Extension을 정의할 수 있게 되었다. 

이렇게 특정 타입에 동작하는 함수를 그 타입의 확장으로 정의하는 것을 권장한다. 이렇게 하면 더 나은 자동완성을 받을 수 있고, 이름더 명확해지며, 구조를 가진 코드를 짜게 될 것이다.


## 제너릭을 사용하는 또다른 Array의 Extension, Filter (필터)
```swift
// 디렉토리 내용을 담고있는 스트링 배열
let exampleFiles = ["README.md", "HelloWorld.swift", "HelloSwift.swift", "FlappyBird.swift"]
```

```swift
// ".swift" 포함하는 파일 찾기
func getSwiftFiles(files: [String]) -> [String] {
	var result: [String] = []
	for file in files {
		if file.hasSuffix(".swift") {
			result.append(file)
		}
	}
	return result
}

getSwiftFiles(exampleFiles)	// ["HelloWorld.swift", "HelloSwift.swift", "FlappyBird.swift"]
```

getSwiftFiles  함수에  String 인자를 추가해 해당 인자를 포함한 파일들을 찾을 수 있도록 일반화 할 수도 있다.  그런데 확장자가 없는 파일이나, 특정 스트링으로 시작하는 파일들을 찾고 싶다면 어떻게 해야 할까?   
이런 종류의 **검색** 을 하기 위해  `filter` 함수를 정의한다. filter 함수 역시 map 처럼 함수를 인자로 받는다. 그리고 이 함수는 `Element -> Bool` 타입을 갖고,  배열의 모든 요소에 대해 결과에 포함될지 말지 결정한다.
```swift
extension Array {
	func filter(includeElement: Element -> Bool) -> [Element] {
		var result: [Element] = []
		for x in self where includeElement(x) {
			result.append(x)
		}	
		return result
	}	
}
```

filter 의 사용은 다음과 같다.
``` swift
func getSwiftFiles(files: [String]) -> [String] {
		files.filter { file in file.hasSuffix(".swift") }
}
```


## Reduce (리듀스)
제너릭을 사용하는 또다른 Array의 Extension인 Reduce를 보도록 하자. 
배열의 모든 정수의 합을 구하는 함수는 다음과 같이 쉽게 정의할 수 있다. 
```swift
func sum(xs: [Int]) -> Int {
	var result: Int = 0
	for x in xs {
		result += x
	}
	return result
}

sum([1, 2, 3, 4])		// 10
```

다음은 배열의 모든 정수의 곱을 구하는 함수이다.
```swift
func product(xs: [Int]) -> Int {
	var result: Int = 1
	for x in xs {
		result = x * result
	}
	return result
}
```

이와 비슷하게 배열의 모든 스트링을 합티는 것이 필요한 경우도 있다.
```swift
func concatenate(xs: [String]) -> String {
	var result: String = ""
	for x in xs {
		result += x
	}
	return result
}
```

아니면 배열의 모든 스트링을 합치는데, 각 요소마다 별도의 헤더라인과 새줄문자를 추가할 수도 있다.
```swift
func prettyPrintArray(xs: [String]) -> String {
	var result: String = "Entries in the array xs: \n"
	for x in xs {
		result = " " + result + x + "\n"
	}
	return result
}
```

위의 네 함수의 공통적 특성(1. result 변수에 할당되는 초기 값, 2. 매 반복마다 result를 갱신하는 기능)을 추상화 할 수 있다.
```swift
extenstion Array {
	func reduce<T>(initial: T, combine: (T, Element) -> T) -> T {
		var result = initial 
		for x in self {
			result = combine(result, x)
		}
		return result
	}
}
```

위에서 정의했던 모든 함수들을 `reduce`로 정의할 수 있다. 
```swift
func sumUsingReduce(xs: [Int]) -> Int {
	return xs.reduce(0) { result, x in result + x }
}

// 클로저 대신 마지막 인자로 연산자를 적는것도 가능하다.
func productUsingReduce(xs: [Int]) -> Int {
	return xs.reduce(1, combine: *)
}

func concatUsingReduce(xs: [String]) -> String {
	return xs.reduce("", combine: +)
}
```

reduce를 사용해 새로운 제너릭 함수를 정의할 수도 있다. 
하나의 배열로 줄이고 싶은 배열의 배열이 있을 때 다음과 같이 for 반복문을 사용하는 함수를 작성할 수 있을 것이다.
```swift
func flatten<T>(xss: [[T]]) -> [T] {
	var result: [T] = []
	for xs in xss {
		result += xs
	}
	return result
}
```

하지만 reduce를 사용한다면 다음과 같이 짧게 작성할 수 있다.
```swift
func flattenUsingReduce<T>(xss: [[T]]) -> [T] {
	return xss.reduce([]) { result, xs in result + xs }
}
```

또한, map과 filter도 reduce를 사용하여 다시 정의할 수 있다.
```swift
extension Array {
	func mapUsingReduce<T>(transform: Element -> T) -> [T] {
		return reduce([]) {	result, x in 
			result + [transform(x)]
		}
	}

	func filterUsingReduce<T>(includeElement: Element -> Bool) -> [Element] {
		return reduce([]) { result, x in 
			return includeElement(x) ? result + [x] : result 
		}
	}
}
```

## 제네릭 vs Any 타입
Generic, Any Type -> 다른 타입의 인자를 받는 함수를 정의하는데 사용 

### Generic vs Any 
```
- Generic 
타입에 유연한 함수를 정의할 수 있다. 타입을 컴파일러가 확인한다.
- Any 타입
 어떤 타입의 값이든 나타낼 수 있는 타입. Swift 의 타입 시스템을 피하는데 사용할 수 있다.
```

인자를 그대로 반환하는 함수. 어떤 인자든 받을 수 있다.
```swift
// Generic 
// 반환 값이 입력값과 같다.
func noOp<T>(x: T) -> T {
	return x
	// return 0 // 타입 오류!
}

// Any
// 반환 값이 입력값과 다를 수 있다. 
func noOpAny(x: Any) -> Any {
	return x	
	// return 0	//  결과 값을 어떤 타입으로 형 변환 해야하는지 알 수 없기 때문에 런타임 예외가 발생할 가능성이 크다. 
}
```

Generic 함수는 극도로 많은 정보를 제공한다. 합성 연산자 `>>>`의 제너릭 버전을 보자.
```swift
infix operator >>> { associativity left }
func >>> <A, B, C>(f: A -> B, g: B -> C) -> A -> C {
	return { x in g(f(x)) }
}
```












