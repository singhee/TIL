# 커링 (Currying)
함수형 프로그래밍 패러다임에서 즐겨 사용되는 `커링(Currying)`기법에 대해 알아보고자 한다. 커링은 **다중 인수를 갖는 함수를 단일 인수로 갖는 함수들의 함수열로 바꾸는 것이다.** 

커링은 주어진 다중 값을 함수로 계산하는 과정과 유사하다. 예를 들어 `f(x, y) = y / x`라는 함수가 주어졌다면 `h(x) = y -> f(x, y)`로 정의할 수 있다. f(2, 3)을 계산하기 위해서 x의 값에 2를 대입한다. 그 후에 y 함수의 결과로 새로운 함수 `g(y) = h(2) = y -> f(2, y) = y / 2`로 정의할 수 있다. 다음으로 y 값에 3을 대입하여 `g(3) = f(2, 3) = 3 / 2`라는 과정을 도출하게 된다.

## 커링 예제 
두 개의 인자를 가지는 함수를 통해 커링 기법으로 발전시켜 본다.
```swift
func add1(x: Int, y: Int) -> Int {
	return x + y	
}
```

위의 함수는 두 개의 인자를 가지고 합계를 반환한다. 클로저를 통해 코드를 다른 방식으로 작성하면 다음과 같다.
```swift
func add2(x : Int) -> (Int -> Int) {
	return { y in return x + y }
}
```
위의 함수는 인자를 하나만 가지고 두번째 인자 y로 예상하는 클로저를 반환한다.

위에서 작성한 두 함수는 각각 다르게 작성하게 된다.

```swift
add(1, 2)
add2(1)(2)
```
첫번째 함수 add1는 클로저 대신 커링된 버전으로 정의할 수 있다.
```swift
func add3(x: Int)(y: Int) -> Int {
	return x + y
}
```

대신 두번째 인자는 인자 명을 명시해주어야 합니다.
```swift
add3(2)(y: 1)
```

다시 돌아가서 앞에서 `f(X x Y) -> Z` 타입의 함수를 커링된 제네릭 함수로 정의할 수 있다.
```swift
func curry<X, Y, Z>(f: (X, Y) -> Z) -> X -> Y -> Z {
	return { x in
		{ y in f(x, y) }
	}
}

func curry<A, B, C, D>(f: (A, B, C) -> D) -> A -> B -> C -> D {
	return { a in { b in { c in f(a, b, c) } } }
}	
```

또한, 여러 인자를 함수로 받아 커링된 버전으로 함수를 정의할 수 있습니다.
```swift
func curry1<X, Y, Z>(f: X -> Y, g: Y -> Z) -> X -> Z {
	return { x in g(f(x)) }
}

infix operator >>> { associativity left }
func >>> <A, B, C>(f: A -> B, g: B -> C) -> A -> C {
	return { x in g(f(x)) }
}

var f: Int -> Int = { $0 + 2 }
var g: Int -> Int = { $0 * 3 }

var myf1 = f >>> g
myf1(2)	// 12

var myf2 = curry1(f, g)
myf2(2)	// 12
```




