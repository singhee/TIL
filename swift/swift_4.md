# Swift 4 변경사항 (Xcode 9 첫 Beta 기준)
Swift는 이제 문법적으로 변경되는 부분은 크게 많지 않다. 그렇기 때문에 Swift 3 버전과 크게 달라진 문법은 많지 않다. 내부적으로 수정되거나 추가된 문법, 기능이 많고 삭제된 부분은 거의 없는 걸로 보면 된다. 

## 프로토콜이 명시된 타입
Objective-C 에서는 특수한 프로토콜을 따르는 인스턴스를 지정하는 방법이 있다. 예를 들어 아래와 같은 프로퍼티를 보자.
```objective-c
@property (strong) id<SomeProtocol> delegate;
```

이렇게 하면 SomeProtocol을 따르는 인스턴스만 `delegate` 에 대입 할 수 있다.
이와 비슷한 것을 Swift 에도 도입하려는 것이 이번에 변경사항으로 나왔다. 아래와 같이 `&` 문자를 이용해 프로토콜을 함께 명시할 수 있다. 

```Swift
var viewController: UIViewController & UITableViewDataSource
```
위와 같은 viewController 프로퍼티가 있다면 이 프로퍼티에는 반드시 UIViewController 의 인스턴스 이면서 동시에 UITableViewDataSource 를 따르는 인스턴스만을 대입 할 수 있게 된다.

`&` 문자는 AND 연산자를 떠올리게 하는데, 타입 선언에서만 사용되니 헷갈리지는 않을 것이다. 

## 사전형과 집합
여러가지 편의 기능이 추가되었지만 대표적인 것만 꼽아보면, 리스트로 사전형을 생성할 수 있게 되었다는 것이다!!

```Swift
let dict = [String : Int](uniqueKeysWithValues: [("a", 111), ("b", 222)])
// ["a": 111, "b": 222]
```
이 예제는 키와 값 순서로 선언한 튜플 배열을 이용해 사전형 데이터를 생성한다.

그리고 다음으로 또 하나 생긴 기능은 **사전형에 기본값**이라는 개념이 생겼다. 기존의 사전형의 경우 엉뚱한 키로 엑세스 하면 nil을 리턴했지만 Swift 4 에서 부터는 아래와 같이 기본값을 설정할 수 있다. 

```Swift
dict["c", default: -1]
// -1
```
참고로 이 예제는 이 앞의 예제가 실행된 것을 가정하고 있다.
그 외에 `Dictionary`과 `Set`에서 사용되는 `filter` 나 `map`이 이제 배열이 아니라 원래의 타입 형태로 리턴되게 바뀌었다.

기타 많은 기능이 추가되었으니 매뉴얼을 확인해보자. 


## 키패스(Key Path) 표기
다음과 같이 새로운 키패스 표기법이 추가되었다. 
```Swift
\Type.propertyName
```
정확하게 예를 들면 다음과 같은 식이다.
```Swift
struct MyType {
  let name: String
  let value: Int
}

let a = MyType(name: "a", value: 100)
a[keyPath: \MyType.name]  // "a"
a[keyPath: \MyType.value]  // 100

let keyPath = \MyType.name
type(of: keyPath)
```
`백슬래시(\)`가 좀 마음에 안들긴 하지만 문법 자체는 나쁘진 않다. 참고로 플레이그라운드에서 마지막의 `type(of:)` 결과과 좀 복잡하게 표시되지만 `String` 타입이라고 알 수 있다. 


## 단방향 범위(Range)
원래 범위 표기는 시작과 끝이 있어야 했다. 예를 들어 1부터 100 까지는 `1...100` 로 표기했다.
그런데 최소나 최대 범위를 표현하기 위해 새로운 문법이 추가되었다. 예를 들어  `1...` 은 1부터 끝 까지라는 범위 표현이다. 반대로 `...100`은 처음부터 100까지라는 범위 표현이다. 루프에 이런 식을 써버리면 아마도 난감하겠지만, 범위내에서 시작이나 끝 지점을 표현할 때는 유용할 것이다. 

## 문자열(String)
문자열에서도 역시나 많은 기능들이 추가되었는데, 몇 가지만 살펴보도록 하겠다.

### 멀티라인 문자열 표기
```Swift
"""
First line.
Second line.
"""
```
마치 Python 에서 본 듯한 문법인데 정말 비슷하다. 이 문자열은 실제로 아래와 같이 만들어진다. 

```Swift
"First Line.\nSecond Line."
```
시작과 끝 부분을 표시하는 따옴표 3개의 위치를 잘 보자. 참고로 멀티라인 문자열 표기법이라 아래와 같이 쓰면 에러가 난다.

```Swift
let str = """This is single line string"""	// Error!
```

### 부분문자열(Substring)
서브스크립트 슬라이스(Subscript Slice)를 통해 생성된 firstWord 의 타입은 Substring 이라는 타입이다. 의미를 보면 알겠지만 원래 문자열의 부분 문자열이라는 의미다. 
```Swift
let string = "this is test"
let index = string.index(of: " ")!  // 4
let firstWord = string[..<index]  // "this"
type(of: firstWord) // Substring.Type
```

### 문자의 갯수(count)
문자 갯수를 알기 위해 이제 문자열 인스턴스의 .characters.count 대신 다시 .count 프로퍼티를 쓸 수 있다.

```Swift
"abc".count == "abc".characters.count
// true
```
