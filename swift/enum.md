# 열거형 (enum)
열거형은 연관된 항목들을 묶어서 표현할 수 있는 타입이다. 열거형은 프로그래머가 정의해준 항목 값 외에는 추가 및 수정이 불가하다. 그렇기 때문에 딱 정해진 값만 열거형의 값으로 속할 수 있다. 열거형은 다음과 같은 경우에 용이하게 사용할 수 있다.

- 제한된 선택지를 주고 싶을 때
- 정해진 값 외에는 입력받고 싶지 않을 때
- 예상된 입력 값이 한정되어 있을 때

스위프트의 열거형은 각 항목별로 값을 가질 수도, 가지지 않을 수도 있다. 예를 들어 C언어는 기본적으로 열거형의 각 항목 값이 정수형으로 지정되지만, **스위프트의 열거형에서는 각 항목이 그 자체로 고유의 값이 될 수 있다.** 따라서 스위프트의 열거형은 실수로 버그가 일어날 가능성을 원천 봉쇄할 수 있다. 물론 열거형 각 항목이 원시 값(Raw Value)형태로[정수, 실수, 문자 등의 타입] 실제 값을 가질 수도 있다. 또는 연관 값(Associated Value)을 사용하여 다른 언어에서 `공용체`라고 불리는 모양의 값의 묶음도 구현할 수 있다. 

## 기본 열거형 
스위프트의 열거형은 `enum` 키워드로 선언 가능하다.

```swift
enum School {
	case primary
	case elementary
	case middle
	case high
	case college
	case university
	case graduate
}
```
위의 School 이라는 이름을 갖는 열거형은 통 7개의 항목을 가진다. 각 항목은 그 자체가 고유의 값이 된다. 항목이 여러 가지라서 케이스별 항목을 나열하기 번거롭다면, 한 줄에 모두 표현해줄 수도 있다.

```swift
// 항목이 여러가지인 열거형을 한 줄로 표현할 경우
enum School {
	case primary, elementary, middle, high, college, university, graduate
}
```

또한 다음과 같이 열거형 변수를 생성하고 값을 할당할 수도 있다.
```swift
var educationLevel: School = School.university
var educationLevel: School = .university		// 동일 표현

educationLevel = .graduate
// 같은 타입인 School 내부의 항목으로만 educationLevel값을 변경해줄 수 있다. 
```

## 원시 값
열거형의 각 항목은 자체로도 하나의 값의 역할을 할 수 있지만, 항목의 원시 값(Raw Value)도 가질 수 있다. 즉, 특정 타입으로 지저오딘 값을 가질 수 있다. 특정 타입의 값을 원시 값으로 가지고 싶다면 열거형 이름 오른쪽에 타입을 명시해주면 된다. 또한 원시 값을 사용하고 싶으면 `rawValue` 프로퍼티를 통해 값을 가져올 수 있다.

```swift
enum School: String {
	case primary = "유치원"
	case elementary = "초등학교"
	case middle = "중학교"
	case high = "고등학교"
	case college = "대학"
	case university = "대학교"
	case graduate = "대학원"
}

let educationLevel: School = School.university

print("저의 최종학력은 \(educationLevel.rawValue) 졸업입니다.")
```
일부 항목만 원시 값을 주는 것도 가능하다. 만약 문자열 형식의 원시 값을 지정해줬다면 각 항목 이름을 그대로 원시 값으로 가지게 되고, 정수형이라면 첫 항목을 기준으로 0부터 1씩 늘어난 값을 갖게 된다.

```swift
enum School: String {
	case primary = "유치원"
	case elementary = "초등학교"
	case middle = "중학교"
	case high = "고등학교"
	case college 
	case university 
	case graduate 
}

print(School.elementary.rawValue)	// elementary

enum Numbers: Int {
	case zero
	case one
	case two
	case ten = 10
}

print("\(Numbers.zero.rawValue), \(Numbers.one.rawValue), \(Numbers.two.rawValue), \(Numbers.ten.rawValue)")
// 0, 1, 2, 10
```

Swift의 enum 에서는 정의되지 않은 원시값을 가지고 생성할 경우 `nil`을 반환하기도 한다. 

## 연관 값
Enum은 연관 값`Associated Values`을 가질 수 있다. 아래 예시는 어떤 API에 대한 에러를 정의한 것이다. invalidParameter 케이스는 필드 이름과 메시지를 가지도록 정의되었다.

```swift
enum NetworkError {
  case invalidParameter(String, String)
  case timeout
}

let error: NetworkError = .invalidParameter("email", "이메일 형식이 올바르지 않습니다.")
```
위의 값을 꺼내올 수 있는 방법으로는 `if-case`또는 `switch`를 활용하는 방법이 있다.
```swift
if case .invalidParameter(let field, let message) = error {
  print(field) // email
  print(message) // 이메일 형식이 올바르지 않습니다.
}

switch error {
case .invalidParameter(let field, let message):
  print(field) // email
  print(message) // 이메일 형식이 올바르지 않습니다.

default:
  break
}
```

### 옵셔널과 Enum
사실, 옵셔널을 `Enum` 으로 정의되어 있다.
```swift
public enum Optional<Wrapped> {
  case none
  case some(Wrapped)
}
```


