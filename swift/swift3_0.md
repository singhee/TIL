# Swift 3.0 의 변경사항
Swift 3.0이 Swift 2.x와 어떤 부분에서 달라졌는지 살펴보자. 

### 1. 커리함수 선언 구문 삭제 
func foo(x: Int)(y: Int) 효과는 제한적 구현이 복잡해지기 때문에 제거해야 한다. 

```swift 
// Before
func curried(x: Int)(y: String) -> Float {
	return Float(x) + Float(y)!
}

// After
func curried(x: Int) -> (String) -> Float {
	return {(y: String) -> Float in 
		return Float(x) + Float(y)!
	}
}
```

### 2. 함수의 매개변수에서 var 삭제
`var`가 많이 있는 경우 함수의 매개변수로 inout과 혼동된다. 따라서 함수 매개변수의 `var`가 삭제되었다. 

```swift
// Before
func foo(var i: Int) {
	
}

// After
func foo(i: Int) {
	var i = i
}
```

### 3. ++/- 연산자 삭제

```swift
// Before
x++

// After
x += 1
```
### 4. Objective-C API를 Swift에 최적화
Swift언의 API설계지침에 따라 형, 메소드, 속성등의 이름이 다음과 같이 최적화 되었다.
* swift_name속성 적용을 일반화
* 중복명을 생략
* 기본 인수를 추가
* 첫번째 인수 레이블 추가
* Boolean 속성 시작은 is를 붙임
* Lowercase values
* compare(_:) -> NSComparisonResult

```swift
// UIBezierPath API in Swift2
class UIBezierPath : NSObject, NSCopying, NSCoding {
	convenience init(ovalInRect: CGRect)
	func moveToPoint(_: CGPoint)
	func addLineToPoint(_: CGPoint)
	func addCurveToPoint(_: CGPoint, controlPoint1: CGPoint, controlPoint2: CGPoint)
	func addQuadCurveToPoint(_: CGPoint, controlPoint: CGPoint)
	func appendPath(_: UIBezierPath)
	func bezierPathByReversingPath() -> UIBezierPath
	func applyTransform(_: CGAffineTransform)
	var empty: Bool { get }
	func containsPoint(_: CGPoint) -> Bool
	func fillWithBlendMode(_: CGBlendMode, alpha: CGFloat)
	func strokeWithBlendMode(_: CGBlendMode, alpha: CGFloat)
	func copyWithZone(_: NSZone) -> AnyObject
	func encodeWithCoder(_: NSCoder)
}
```

```swift
class UIBezierPath : NSObject, NSCopying, NSCoding {
	convenience init(ovalIn rect: CGRect)
	func move(to point: CGPoint)
	func addLine(to point: CGPoint)
	func addCurve(to endPoint: CGPoint, controlPoint1 controlPoint1: CGPoint, controlPoint2 controlPoint2: CGPoint)
	func addQuadCurve(to endPoint: CGPoint, controlPoint controlPoint: CGPoint)
	func append(_ bezierPath: UIBezierPath)
	func reversing() -> UIBezierPath
	func apply(_ transform: CGAffineTransform)
	var isEmpty: Bool { get }
	func contains(_ point: CGPoint) -> Bool
	func fill(_ blendMode: CGBlendMode, alpha alpha: CGFloat)
	func stroke(_ blendMode: CGBlendMode, alpha alpha: CGFloat)
	func copy(with zone: NSZone = nil) -> AnyObject
	func encode(with aCoder: NSCoder)
}
```

### 5. C-style for문 제거
Swift 3.0은 `-`와 `++`가 제거됨에 따라 C-Style의 반복문은 없어졌다. 새로운 for문은 Swift 2.2부터 구현되어 있다.
```swift
// Before
for var i = 0 ; i < 10 ; i++ {

}

// After
for i in 0..<10 {

}
```

```swift
// Before
for var i = 0 ; i <= 10 ; i++ {

}

// After
for i in 0...<10 {

}
```

### 6. 선택적 배열을 위한 .Lazy flatMap 추가
현재 Swift의 표준라이브러리에서는 두가지 flatMap이 있다.
```swift
// 필수사항
[1, 2, 3]
.flatMap { n in n..<5 } 
// [1, 2, 3, 4, 2, 3, 4, 3, 4]
// 선택적
(1...10)
.flatMap { n in n % 2 == 0 ? n/2 : nil }
// [1, 2, 3, 4, 5]
```

```swift
[1, 2, 3]
.lazy
.flatMap { n in n..<5 }
// LazyCollection<FlattenBidirectionalCollection<LazyMapCollection<Array<Int>, Range<Int>>>>
(1...10)
.lazy
.flatMap { n in n % 2 == 0 ? n/2 : nil }
// [1, 2, 3, 4, 5]
```


### 7. 형 장식을 위한 inout선언 위치 조절
```swift
// Before
func foo(inout a:Int, inout b:Int) {
}

// After
func foo(a: inout Int, inout b:Int) {
}
```

### 8. 여러가지 패턴의 case레이블 변수
아래와 같이 여러가지 패턴이 포함되는 경우 오류가 발생하는데 이런 오류가 발생하지 않도록 변경되었다.

```swift
enum MyEnum {
	case Case1(Int,Float)
	case Case2(Float,Int)
}

switch value {
case let .Case1(x, 2), let .Case2(2, x):
	print(x)
case .Case1, .Case2:
	break
}
```

### 9. @noescape에서 @autoclosure로 이동
@noescape와 @autoclosure를 추가하는 위치가 변경되었다. 

```swift
// befor
func f(@noescape fn :() -> ()) {}
// after
func f(fn : @noescape () -> ()) {}
```

```swift
// befor
func f2(@autoclosure a : () -> ()) {}
// after
func f2(a : @autoclosure () -> ()) {}
```







