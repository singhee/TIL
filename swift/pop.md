# Prorocol Oriented Programming (POP)
Swift에서는 지난 수십 년간 사용되었던 개발 패턴인 객체 지향 프로그래밍(OOP)이 프로토콜 지향 프로그래밍(POP)으로 대체되고 있다. Class를 통한 OOP가 Struct와 Protocol을 통한 POP으로 바뀌고 있다는 것이다. OOP의 핵심 중 하나는 바로 상속이다. Swift에서 Class는 상속이 가능하고, Struct나 Enum은 상속이 불가능하다. 이 때문에 Swift코드에서 거의 대부분의 데이터 구조는 Class로 작성되었다. 예를 들면, UIView를 상속하는 임의의 View Class라든지, UIViewController를 상속하는 임의의 Controller Class를 선언했다. View나 Controller뿐만 아니라 Model도 Class로 선언하는 경우가 많았다. 

```Swift 
class ViewController: UIViewController {
	override func viewDidLoad() {
		super.viewDidLoad()
	}

	override func didReceiveMemoryWarning() {
		super.didReceiveMemoryWarning()
	}
}
```
하지만 Class와 같은 Reference Type은 단일 원본이 여기 저기 참조되기 때문에 그리 권유되는 방법은 아니다. 물론 기능적으로 하나의 원본을 여러 군데에서 사용해야 하는 경우도 있다. 하지만 Multi-threaded 환경에서는 한 원본을 두고 여러 작업이 동시에 진행되게 되면 원본 데이터가 꼬일 수 있다. 한 스레드가 원본 데이터를 불러오고 있는데, 다른 스레드가 원본 데이터를 수정하고 있는 경우처럼이다. 이에 굳이 하나의 원본으로 작업할 필요가 없다면 가급적이면 Value Type인 Struct나 Enum을 사용하는 것이 권장되고 있다. 

다만 문제는 위에서 언급한 것처럼 Struct나 Enum은 객체지향의 핵심인 상속이 불가능하다는 약점이 있다. 이 부분을 해결해주는 것이 바로 Protocol Extension이다. 원래 Protocol은 기능을 수행하는 코드는 넣을 수 없다. 그냥 Protocol을 따른다고 선언하는 Type이 구현해야할 코드들의 '껍데기'를 정의해주는 일종의 설계도였을 뿐이다. 이 때문에 어느 Protocol을 따른다고 선언한 2개의 Class나 Struct가 있을 때 둘 다 Protocl에서 정의한 메서드를 구현하기는 해도, 내용과 기능은 완전히 다른 메서드가 될 소지가 높았다. 
```Swift 
// 구현해야할 API의 껍데기만 제시할 수 있는 protocol
protocol SomeProtocol {
	var propertyOne: Int { get set }
	var propertyTwo: String { get }

	func methodOne() -> Int
	func methodTwo(param: String) -> (String, String)
}

// 아래 ClassOne과 StructOne 모두 같은 SomeProtocol을 따름에도 프로퍼티와 메서드의 값과 기능 자체는 다름
 
class ClassOne: SomeProtocol { 
	var propertyOne: Int = 1
	var propertyTwo: String = "Two"

	func methodOne() -> Int {
		return 1
	}

	func methodTow(param: String) -> (String, String) {
		return ("Hi", "Hello")
	} 
}

struc StructOne: SomeProtocol {
	var propertyOne: Int = 101
	var propertyTwo: String = "TwoHundred"

	func methodOne() -> Int {
		return 101
	}

	func methodTwo(param: String) -> (String, String) {
		return ("Good", "Day")
	}
}
```

하지만 Swift2 에서부터 Protocol Extension에는 실제 값을 계산해내고, 기능을 하는 메서드를 구현할 수 있게 되었다. 일반 Class처럼 Property와 Method를 정의할 수 있게 되었다는 뜻이다. 이를 통해 진정한 Protocol Oriented Programming이 가능해졌다. 참고로 Protocol Extension이 그러하듯이 Stored Property는 올 수 없고 오직 Computed Property만 구현 할 수 있다. 

```Swift 
// extension을 통해 protocol에 기능 코드 추가
extension SomeProtocol { 
	var propertyThree: Int {
		return propertyOne * 2
	}

	func methodThree() -> String {
		let name = "Jack"
		return name
	}
}
```

## POP의 장점
### 1. 가벼움과 보안성
Class처럼 모든 API를 가져올 필요없이 Protocol을 통해 필요한 API만 가져올 수 있다. 아래 예시는 Class 인스턴스일 때와 Protocol로 접근할 때의 차이를 보여준다. Class인스턴스로 접근하면 Class의 모든 API에 접근할 수 있음에 반해, Protocol로 접근할 경우 Protocol에서 정의한 API만 가져가게 된다. 이에 더 가볍고 보안성이 더 높다고 볼 수 있다. 
```Swift 
protocol Eat {
	func eat()
}

class Human: Eat {
	func speak() {
		print("Hello")
	}

	func eat() {
		print("Yum Yum")
	}
}

// Class로 접근
let human = Human()
human.speak()
human.eat()

// Protocol로 접근
let human2: Eat = Human()
// human2.speak() // 에러, 접근불가
human2.eat()
```


### 2. Value Type 상속
Protocol Extension을 통해 Struct, Enum과 같은 Value Type도 상속효과를 줄 수 있게 되었다. 이를 통해 상속이 필요한 프로그램을 짤 때 Class가 아닌 Struct나 Enum을 사용할 수 있게 되었고, 멀티스레드 환경에서 Thread Safety도 높일 수 있게 된다. 
```Swift 
// OOP
class HumanClass {
	func greet() {
		print("Hi there!")
	}
}

class SingerClass: HumanClass {
	let canSing = true
}

let stevie = SingerClass()
stevie.greet() 	// Hi there!


// POP
protocol GreetProtocol {

}

extension GreetProtocol {
	func greet() {
		print("Hi there!")
	}
}

struct SingerStruct: GreatProtocol {
	let canSing = true
}

let george = SingerStruct()
george.greet()	// Hi there!
```

### 3. 다중 상속과 수평 확장
하나의 Superclass만 상속 가능한 Class와는 달리 여러 Protocol을 상속받을 수 있다. 또한 OOP처럼 수직적인 확장 구조가 아닌 수평적인 확장 구조를 지닌다. 이를 통해 수직 구조를 고려할 필요 없이 필요에 따라 레고 블럭처럼 이것저것 제약 없이 붙일 수 있게 되었다. 아래는 이와 같은 특성을 나타내고자 단순화시킨 예이다. 여기에 `Protocol Extension`을 통해 기능을 하는 코드를 넣어주면 상황에 따라 유연하게 구성할 수 있게 된다.
```Swift 
// Protocols
protocol CanShootThrees {}
protocol CanBlock {}
protocol CanRebound {}

// Structs
struct PureShooter: CanShootThrees {}
struct DefensiveForward: CanBlock, CanRebound {}
struct SuperStar: CanShootThrees, CanBlock, CanRebound {}
```
### 4. Generics 더하기
POP에 자료형의 구애를 받지 않는 Generics를 얹으면 더욱 강력해진다.

## `where Self:`
보통 `Extension`뒤에 `where Self:`자료형을 통해 추가적인 조건을 지정할 수 있다. Protocol Extension에서도 마찬가지이다. 아래 예시는 Protocol을 확장하며 추가 조건으로 Protocol을 제시한 구조이다. 'A를 따른다고 한 것들 중에 특별히 B까지 따르는 것들은 추가로 이것들을 더 주겠다'는 의미이다. 즉, Offense 프로토콜을 따른다고 선언한 Type중 추가적으로 Defense 프로토콜까지 따르는 Type에게만 showOff()메서드가 추가되도록 하고 있다.

```
// Protocol과 Extension
protocol Offense {
}

protocol Defense {
}

extension Offense where Self: Defense {
	func showOff() {
		print("I can play both offense and defense!")
	}
}

// Struct
struct Scorer: Offense {
}

struct DefensiveSpecialist: Defense {
}

struct AllAroundPlayer: Offense, Defense {
}

// Instance
let iverson = Scorer()
let bowen = DefensiveSpecialist()
let jordan = AllAroundPlayer()

// iverson.showOff() // 에러
// bowen.showOff()	// 에러
jordan.showOff()	// "I can play both offense and defense!"
```

## POP 실전활용
Swift가 점차 진화하면서 기존 빌트인 메서들도 진화하고 있다. 기존에 class나 struct였던 것들이 POP의 힘으로 struct 또는 protocol로 정의되고 있다. 다음은 RayWenderlich에 나온 예시이다.
```Swift
let numbers = [10, 20, 30, 40, 50, 60]
let slice = numbers[1...3]
let reversedSlice = slice.reversed()

let answer = reversedSlice.map { $0 * 10 }
print(answer)
```
numbers라는 Array를 정의하고, 이 중에서 특정 값만 가져온 다음 역순으로 변형하고 각 값에다가 10을 곱한 Array를 도출하고 있다. 그런데 `slice`, `reversedSlice`의 자료형은 무엇일까? 얼핏 봐서는 모두 Array인 것처럼 보인다. 하지만 Alt + 클릭 또는 아래 Mirror를 통해 자료형을 확인해 보면 다음과 같다.

```
print(Mirror(reflecting: slice).subjectType)
// ArraySlice<Int>
print(Mirror(reflecting: reversedSlice).subjectType)
// ReversedRandomAccessCollection<ArraySlice<Int>>
```
여기의 뒤에는 POP이 숨어 있다. 











