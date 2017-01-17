## Nil 병합 연산자 (Nil Coalescing Operator)
스위프트에서 nil은 주요 개념이다. 이런 nil은 효율적으로 처리하기 위한 다양한 방법들이 존재하는데, 그 중 하나가 **Nil 병합 연산자이다**. Nil 병합 연산자는 옵셔널을 사용할 때 아주 유용하게 사용될 수 있는 연산자이다. 


`anOptionalInt`라고 하는 옵셔널 값이 있고 이 변수에 옵셔널이 아닌 값을 대입하려고 한다고 가정해 보자. 만약 옵셔널 값이 `nil`이면 어떤 값을 할당하는 과정은 다음과 같이 나타낼 수 있을 것이다.
```swift
var anOptionalInt: Int? = 10

var anInt: Int = 0

if anOptionalInt != nil {
	anInt = anOptionalInt!
}
```
위의 코드는 하는 일에 비해 코드가 불필요하게 길다는 것을 알 수 있다. 위의 코드는 보다 간결하게 작성할 수 있다.

### 삼원 조건 연산자
C언어처럼 스위프트에도 삼원 조건 연산자가 있다. 이 연산자를 사용하는 것은 보통 읽기가 어려워진다는 관점에서 사용하는데 반대하는 의견이 많다.

```swift
var anOptionalInt: Int? = 10

var anotherOptional = (anOptionalInt != nil ? anOptionalInt! : 0)
```
첫 번째 예제보다 코드의 길이가 훨씬 짧아졌지만, 코드의 가독성은 매우 떨어진다는 것이 단점입니다.

### Nil 병합 연산자
스위프트에서는 이런 상황에 사용할 목적의 전용 연산자로 `Nil 병합 연산자(nil coalescing operator)`가 있다.

```swift
var anOptionalInt: Int? = 0
var anotherOptional = anOptionalInt ?? 0
```
삼원 연산자에 대비하면 상대적으로 가독성도 좋고 코드의 길이도 짧아진다. 