# 옵셔널 (Optional)
옵셔널 `Optional` 은 Swift 가 가지고 있는 가장 큰 특징 중 하나로, 값이 있을 수도 있고 없을 수도 있는 것을 나타낸다. 

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
let optionalEmail: String? = "sangheeyoon@gmail.com"
let requiredEmail: String = optionalEmail // 컴파일 에러!
```
requiredEmail 변수는 옵셔널이 아닌 String이기 때문에 항상 값을 가지고 있어야 한다. 반면에, optionalEmail은 옵셔널로 선언된 변수이기 때문에 실제 코드가 실행되기 전까지는 값이 있는지 없는지 알 수 없다. 따라서 Swift 컴파일러는 안전을 위해 requiredEmail에는 옵셔널로 선언된 변수를 대입할 수 없게 만들었다.

## 옵셔널 바인딩 (Optional Binding)
그렇다면 옵셔널의 값을 가져오고 싶은 경우엔 어떻게 하면 될까? 이럴 때 사용하는 것이 바로 옵셔널 바인딩 `Optional Binding` 이다. 

옵셔널 바인딩은 옵셔널의 값이 존재하는지를 검사한 뒤, 존재한다면 그 값을 다른 변수에 대입시켜준다. `if let` 또는 `if var` 를 사용하는데, 옵셔널의 값을 벗겨서 값이 있다면 `if`문 안으로 들어가고, 값이 `nil`이라면 그냥 통과하게 된다.

예를 들어, 아래의 코드에서 optionalEmail에 값이 존재한다면 email이라는 변수 안에 실제 값이 저장되고, `if`문 내에서 그 값을 사용할 수 있다. 만약 optionalEmail이 `nil`이라면 `if`문이 실행되지 않고 넘어간다.

```swift
if let email = optionalEmail {
  print(email) // optionalEmail의 값이 존재한다면 해당 값이 출력된다.
}
// optionalEmail의 값이 존재하지 않는다면 if문을 그냥 지나친다.
```
하나의 `if`문에서 `콤마(,)` 로 구분하여 여러 옵셔널을 바인딩할 수 있다. 이곳에 사용된 모든 옵셔널의 값이 존재해야 `if`문 안으로 진입한다.
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

옵셔널을 바인딩할 때 `,`를 사용해서 조건도 함께 지정할 수 있다. , 이후의 조건절은 옵셔널 바인딩이 일어난 후에 실행된다. 즉, 옵셔널이 벗겨진 값을 가지고 조건을 검사하게 된다.

```swift
var optionalAge: Int? = 22

if let age = optionalAge, age >= 20 {
  // age의 값이 존재하고, 20 이상입니다.
}

```































