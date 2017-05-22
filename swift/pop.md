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

하지만 Swift2 에서부터 Protocol Extension에는 실제 값을 계산해내고, 기능을 하는 메서드를 구현할 수 있게 되었다. 일반 Class처럼 Property와 Method를 정의할 수 있게 되었다는 뜻이다. 이를 통해 진정한 Protocol Oriented Programming이 가능해졌다. 참고로 Protocol Extension이 그러하듯이 Stored Property는 올 수 없고 오직 Computed Property만 구현 할 수 있다. 

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

### 3. 다중 상속과 수평 확장
하나의 Superclass만 상속 가능한 Class와는 달리 여러 Protocol을 상속받을 수 있다. 또한 OOP처럼 수직적인 확장 구조가 아닌 수평적인 확장 구조를 지닌다. 이를 통해 수직 구조를 고려할 필요 없이 필요에 따라 레고 블럭처럼 이것저것 제약 없이 붙일 수 있게 되었다. 

### 4. Generics 더하기
POP에 자료형의 구애를 받지 않는 Generics를 얹으면 더욱 강력해진다.
