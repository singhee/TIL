# 지연 저장 프로퍼티 (Lazy)
인스턴스가 생성될 때 프로퍼티에 값이 필요 없다면 프로퍼티를 옵셔널로 선언해줄 수 있다. 그런데 이와는 다른 용도로 필요할 때 값이 할당되는 **지연 저장 프로퍼티(Lazy Stored Properties)** 가 있다. 지연 저장 프로퍼티는 호출이 있어야 값을 초기화 하며 `lazy` 키워드를 사용한다.

상수는 인스턴스가 완전히 생성되기 전에 초기화되어야 하므로 필요할 때 값을 할당하는 지연 저장 프로퍼티에는 맞지 않다. 따라서 지연 저장 프로퍼티는 `var`키워드를 사용하여 변수로 정의한다.

지연 저장 프로퍼티는 주로 복잡한 클래스나 구조체를 구현할 때 많이 사용된다. 클래스 인스턴스의 저장 프로퍼티로 다른 클래스 인스턴스나 구조체 인스턴스가 할당되어야 할 때가 있다. 이럴 때 인스턴스를 초기화하면서 저장 프로퍼티로 쓰이는 인스턴스들이 한 번에 생성되어야 한다면? 또, 굳이 모든 저장 프로퍼티를 사용할 필요가 없다면? 이 질문에 대한 답이 바로 지연 저장 프로퍼티 이다. 지연 저장 프로퍼티를 잘 사용하면 불필요한 성능저하나 공간 낭비를 줄일 수 있다. 지연 저장 프로퍼티를 선언하는 방법은 다음과 같이 할 수 있다.

```swift
struct CoordinatePoint {
	var x: Int = 0 
	var y: Int = 0
}

class Position {
	lazy var point: CoordinatePoint = CoordinatePoint()
	let name: String

	init(name: String) {
		self.name = name
	}
}

let sangheePosition: Position = Position(name: "sanghee")

// 이 코드를 통해 point 프로퍼티로 처음 접근할 때 point 프로퍼티의 CoordinatePoint가 생성된다.
print(sangheePosition.point)	// x: 0, y: 0
```
위의 `lazy` 키워드를 사용하는 것 만으로도 다양한 효과를 낼 수 있다.
- 구체적인 값을 미리 지정하지 않고도 지연변수(lazy var)로 지정된 `point`에 접근하면 "그 시점"에 디폴트 값이 연산되고 반환된다. 그 이후에 다시 이 변수에 접근하면 이미 생성되어 저장되어 있는 값이 반환된다.
- `point`를 읽기 전에 명시적으로 어떤 값을 지정하면, 연산 비용이 많이 드는 디폴트 값은 아예 연산을 수행하지도 않는다. 
- 아무도 `point`프로퍼티에 접근하지 않는다면 이 디폴트 값을 만들어 내는 코드도 아예 수행되지 않는다.

## 클로저를 활용한 초기화
보통의 프로퍼티와 마찬가지로 지연 변수(lazy vars)에도 클로저를 사용하여 디폴트 값을 지정할 수 있다. 이 방법은 디폴트 값을 연산하는데 한 줄로 되지 않고 여러 라인의 코드가 필요한 경우에 유용하다.

```swift
class Avatar {
	static let defaultSmallSize = CGSize(width: 64, height: 64)

	lazy var smallImage: UIImage = {
		let size = CGSize(
			width: min(Avatar.defaultSmallSize.width, self.largeImage.size.width),
			height: min(Avatar.defaultSmallSize.height, self.largeImage.size.height)
		)
		return self.largeImage.resizedTo(size)
	}()

	var largeImage: UIImage

	init(largeImage: UIImage) {
		self.largeImage = largeImage
	}
}
```
위의 경우에 `smallImage`변수는 지연(lazy)된 것이므로 클로저 내부에서 "`self`를 참조할 수 있다."

지연(lazy)의 뜻은 그 연산 처리를 하는 코드가 `self`가 완전히 초기화 과정을 끝낸 시점 이후에 호출된다는 것이기 때문에 `self`를 참조하는 것에 아무런 문제가 없다는 것이다. 일반 프로퍼티에서는 초기화 과정에 코드가 실행되므로 `self`를 참조할 수 없다.

위의 예에서 처럼 `lazy`변수의 초기화에서 사용되는 클로저에는 `@noescape`가 자동적으로 적용된다. 따라서 클로저 내부에서 `[unowned self]`와 같은 캡쳐 과정이 불필요하며 순환참조가 발생할 걱정을 할 필요가 없다는 뜻이다. 


> **다중 스레드와 지연 저장 프로퍼티**
> 다중 스레드 환경에서 지연 저장 프로퍼티에 동시다발적으로 접근할 때에는 한 번만 이니셜라이즈된다는 보장은 없다. 생성되지 않은 지연 저장 프로퍼티에 비슷한 시점에 많은 스레드가 접근한다면, 여러 번 이니셜라이즈될 수 있다. 

## 지연 상수? (lazy let)
lazy 는 상수는 허용하지 않는다. 즉, `lazy let`은 지원되지 않는다. `lazy`를 구현하기 위해서는 초기화 과정에서는 값이 없는 변수를 만들었다가 나중에 이 변수에 접근하는 시점에 변경하는 방법을 사용해야 하기 때문에 상수로는 불가능하다. 
하지만, 상수 `let`의 흥미로운 특징 중의 한 가지는 전역으로 선언된 상수, 타입 프로퍼티(`static let`을 사용한 경우)는 자동으로 지연(lazy)처리 된다는 점이다. (거기에다 쓰레드 세이프하기 까지 하다.)

```swift
// 전역 상수. 쓰레드 세이프하게 지연되어 생성됩니다.
let foo: Int = {
  print("Global constant initialized")
  return 42
}()

class Cat {
  static let defaultName: String = {
    print("Type constant initialized")
    return "Felix"
  }()
}

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
  func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    print("Hello")
    print(foo)
    print(Cat.defaultName)
    print("Bye")
    return true
  }
}
```
위 코드는 “Hello”를 출력하고, 전역 상수 foo를 42로 초기화하며, 그 이후에 타입 상수를 초기화 하여 “Felix”를 출력한 후에 “Bye”를 출력한다. 상수 foo와 고양이 이름 “Felix”는 이 값에 접근하는 경우에만 초기화 된다.