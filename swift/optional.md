# 옵셔널 (Optional)
옵셔널 `Optional` 은 Swift의 특징 중 하나인 안정성(Safe)을 문법적으로 담보하는 기능으로, '선택적인' 즉, '값이 있을 수도 있고 없을 수도 있는 것'을 나타낸다. 이는 변수 또는 상수의 값이 꼭 있다는 것을 보장할 수 없다(nil) 는 것을 의미한다. 

예를 들어, 문자열에 값이 있으면 `"가나다"` 가 될 것이다. 그럼, 문자열에 값이 없다면 `""` 일까? 아니다! `""` 도 엄연히 값이 있는 문자열이다. 정확히는 '값이 없다' 가 아니고 '빈 값' 이다. 값이 없는 문자열을 `nil` 이라 표현한다.

또 다른 예로는, 정수형을 예로 들 수 있겠다. 정수형의 값이 있으면 `123` 과 같은 값이 있을 것이다. 값이 없다면 `0` 일까? 마찬가지고 `0` 은 `0` 이라는 숫자 값이다. 이 경우에도 값이 없는 정수는 `nil` 이다. 

마찬가지로 빈 배열이나 빈 딕셔너리라고 해서 '값이 없는' 것은 아니다. 다만, '비어 있을' 뿐이다. 배열과 딕셔너리의 경우에도 '없는 값'은 `nil` 이다.

이렇게, 값이 없는 경우를 나타낼 때에는 `nil`을 사용한다. 그렇다고 해서 모든 변수에 `nil`을 넣을 수 있는 것은 아니다. 예로, 우리가 위에서 정의한 name이라는 변수에 `nil`을 넣으려 하면 에러가 발생한다.

```swift
var name: String = "가나다"
name = nil // Nil cannot be assigned to type 'String'
```
값이 있을 수도 있고 없을 수도 있는 변수를 정의할 때에는 type anotation 에 `?` 를 붙여야 한다.
이렇게 정의한 변수를 바로 옵셔널 `Optional` 이라고 하며, 옵셔널에 초깃값을 지정하지 않으면 기본값은 `nil` 이 된다.

```swift
var email: String?
print(email) // nil

email = "sangheeyoon@gmail.com"
print(email) // Optional("sangheeyoon@gmail.com")

```
옵셔널로 정의한 변수는 옵셔널이 아닌 변수와는 다르다. 예를 들어, 아래와 같은 코드는 사용할 수 없다.

```swift
let optionalEmail: String? = "sangheeyoon@gmail.com" // let optionalEmail: Optional<String> 처럼 제네릭을 사용하여도 된다.
let requiredEmail: String = optionalEmail // 컴파일 에러!
```
requiredEmail 변수는 옵셔널이 아닌 String이기 때문에 항상 값을 가지고 있어야 한다. 반면에, optionalEmail은 옵셔널로 선언된 변수이기 때문에 실제 코드가 실행되기 전까지는 값이 있는지 없는지 알 수 없다. 따라서 Swift 컴파일러는 안전을 위해 requiredEmail에는 옵셔널로 선언된 변수를 대입할 수 없게 만들었다.

## 옵셔널은 언제 쓰이는가?
그렇다면, 옵셔널은 어떤 상황에 쓰일까? 왜 굳이 변수에 nil이 있음을 가정해야 할까? 옵셔널은 함수의 처리 실패에 대응할 수 있다. 즉, 우리가 만든 함수에 전달되는 전달인자의 값이 잘못된 값일 경우 제대로 처리하지 못했음을 nil을 반환하여 표현할 수 있는 것이다. 또한 매개변수를 굳이 넘기지 않아도 된다는 뜻으로 매개변수의 타입을 옵셔널로 정의하기도 한다. 

## 옵셔널의 정의
옵셔널의 정의를 살펴보게 되면 아래와 같이 열거형으로 구현되어 있다는 사실을 알 수 있다. 
```swift
public enum Optional<Wrapped> : ExpressibleByNilLiteral {
	case none
	case some(Wrapped)

	public init(_ some: Wrapped)
	/// ...
}
```
옵셔널은 값을 갖는 케이스(some)와 그렇지 못한 케이스(none) 두 가지로 정의되어 있다. 옵셔널이 값을 가지면 some의 연관 값인 `Wrapped`에 값이 할당된다. 즉, **값이 옵셔널이라는 열거형의 방패막에 보호되어 Wrapped되어 있는 모습이라는 것이다.**


## 옵셔널 추출
열거형의 `some` 케이스로 Wrapped된 옵셔널의 값을 옵셔널이 아닌 값으로 추출하는 옵셔널 추출`(Optional Unwrapping)`방법에 대해 알아보도록 할 것이다.

### 옵셔널 강제 추출 (Forced Unwrapping)
옵셔널 강제 추출 방식은 옵셔널 값을 추출하는 가장 간단하면서도 **가장 위험한 방법**이다. 그 이유는 런타임 오류가 일어날 가능성이 가장 높기 때문이다. 강제 추출 방법은 옵셔널 값의 뒤에 느낌표 `!`를 붙여주면 값을 강제로 추출하여 반환해준다. 만약, 강제 추출 시 옵셔널이 `nil`이라면 런타임 오류가 발생하게 된다.  

```swift
var myName: String? = "sanghee"
var sanghee: String = myName!	// 옵셔널이 아닌 변수에는 옵셔널 값이 들어갈 수 없기 때문에 추출해서 할당해주어야 한다.

myName = nil
sanghee = myName!	// 런타임 오류

// 조건문을 사용하여 안전하게 처리할 수 있다.
if myName != nil {
	print("My name is \(myName!)")
} else {
	print("myName == nil")
}
```

### 옵셔널 바인딩 (Optional Binding)
위에서 처럼 if 구문을 통해 옵셔널 변수가 `nil`인지 먼저 확인하고 옵셔널 값을 강제 추출하는 방법은 다른 프로그래밍 언어에서 `NULL`값을 체크하는 방식과 비슷하다. 하지만, 이는 옵셔널을 사용하는 의미가 없어진다. 그래서 스위프트에서는 조금 더 안전하고 세련된 방법으로 `옵셔널 바인딩(Optional Binding)`을 제공한다. 

옵셔널 바인딩은 옵셔널에 값이 있는지 확인할 때 사용한다. 옵셔널의 값이 존재하는지를 검사한 뒤, 존재한다면 그 값을 다른 변수에 할당해서 옵셔널이 아닌 형태로 사용할 수 있게 해준다. `if let(임시 상수)` 또는 `if var(임시 변수)` 를 사용하는데, 옵셔널의 값을 벗겨서 값이 있다면 `if`문 안으로 들어가고, 값이 `nil`이라면 그냥 통과하게 된다.

예를 들어, 아래의 코드에서 optionalEmail에 값이 존재한다면 email이라는 변수 안에 실제 값이 저장되고, `if`문 내에서 그 값을 사용할 수 있다. 만약 optionalEmail이 `nil`이라면 `if`문이 실행되지 않고 넘어간다.

```swift
if let email = optionalEmail {
  print(email) // optionalEmail의 값이 존재한다면 해당 값이 출력된다.
}
// optionalEmail의 값이 존재하지 않는다면 if문을 그냥 지나친다.

if var email = optionalEmail {
	email = "yoonsanghee@gmail.com"
	print(email)
}else {
	print("email == nil")
}
```

하나의 `if`문에서 `콤마(,)` 로 구분하여 여러 옵셔널의 값을 추출할 수도 있다. 단, 바인딩 하려는 옵셔널 중 하나라도 값이 없다면 해당 if문에 진입할 수 없다.
```swift
var optionalName: String? = "가나다"
var optionalEmail: String? = "sangheeyoon@gmail.com"

if let name = optionalName, email = optionalEmail {
  // name과 email 값이 존재
}
```
> 코드가 너무 길 경우에는, 이렇게 여러 줄에 걸쳐서 사용하는 것이 바람직하다.
```swift
if let name = optionalName,
  let email = optionalEmail {
  // name과 email 값이 존재
}
```

옵셔널을 바인딩할 때 `,`를 사용해서 조건도 함께 지정할 수 있다. `,` 이후의 조건절은 옵셔널 바인딩이 일어난 후에 실행된다. 즉, 옵셔널이 벗겨진 값을 가지고 조건을 검사하게 된다.

```swift
var optionalAge: Int? = 22

if let age = optionalAge, age >= 20 {
  // age의 값이 존재하고, 20 이상입니다.
}

```

옵셔널 바인딩은 [옵셔널 체이닝](https://github.com/singhee/TIL/blob/master/swift/optional_chaining.md)과 환상의 결합을 이룬다. 

### 암시적 추출 옵셔널 (Implicitly Unwrapped Optionals)
옵셔널 형식의 유효성을 확인하고 값을 추출하는 작업이 반복되는 경우에는 불필요한 코드의 양이 늘어나고 지루한 작업이 된다. 옵셔널 형식이 항상 유효한 값을 가지는 경우에는 자료형 뒤에 `?` 대신 `!`를 붙여서 암시적으로 추출되는 옵셔널 형식으로 선언할수 있다.
```
var myName: String! = "sanghee"
print("name: \(myName)")
```

이 형식은 클래스의 initializing에 주로 사용된다. 암시적으로 추출되는 옵셔널 형식은 실제 코드에서 사용될때는 값 형식 또는 참조 형식과 동일하게 사용할 수 있다. 이 형식으로 선언된 변수와 상수는 항상 유효한 값을 가지고 있고 선언시 느낌표를 통해 값에 접근할때마다 암시적으로 값을 추출해서 사용하도록 선언했기 때문에 강제추출처럼 값에 접근할때 강제 추출 연산자(!)를 사용할 필요가 없다.

옵셔널을 사용할 때는 강제 추출 또는 암시적 추출 옵셔널을 사용하기보다는 옵셔널 바인딩, nil 병합연산자, 옵셔널 체이닝 등의 방법을 사용하는 편이 훨씬 안전하며 스위프트 지향점에 가장 부합하는 방법이다. 
 


























