# Swift 4 에서의 새로운 reduce의 기능
기존의 reduce는 아래와 같이 사용하였다. 
```Swift
func reduce<T>(_ initial: T, _ combine: (T, Element) throws -> T) rethrows -> T
```

Swift 4.0부터는 다음과 같이 사용된다고 한다. 
```Swift
func reduce<T>(into initial: T, _ combine: (inout T, Element) -> ()) -> T
```

이렇게 바뀌게 된 것은 reduce가 가지고 있던 문제를 해결하기 위함이다. 그 문제점은 예를들면, reduce를 사용해 Array를 생성한다고 가정해 보자. `[2, 3, 5]` 라는 배열을 `[2, 2+3, 2+3+5]`처럼 해당 요소와 지금까지의 총 요소값을 저장하는 배열을 만든다고 하면, 다음과 같이 작성할 수 있다. 
```Swift
[2, 3, 5].reduce([]) { $0 + [($0.last ?? 0) + $1] }
```

그렇지만 reduce를 사용하여 Dictionary 를 만들려고 하면 쉽지 않다. 예로 사용자 정보를 가진 User의 id키를 만들기 위해 `[Int: User]`형태의 사전을 만들려고 한다면 아래와 같이 작성해야 한다. 
```Swift
users.reduce([Int: User]()) {
    var result: [Int: User] = $0
    result[$1.id] = $1
    return result
}
```
하지만 이는 5줄이나 작성해야 하고 쓰기도 어렵고, 가독성이 좋지 못하고 제일 중요한 성능도 별로이다. 즉, 배열로 만들어사용하면 간단하게 한줄이면 가능하지만 사전형태로 만들려면 5줄이상으로 코드가 크게 증가한다. `id->User`로 맵핑한다는 형태로 처리하는 것인데 `(result[$1.id] = $1)`의 클로저식 안에 처리가 뒤죽박죽되어 있어 무슨 코드인지 알아보기 힘들다. 성능이 나쁜 이유는 최적화가 제대로 실행되지 않기 때문에 몇배 작업이 복잡해지기 때문이다. O(N) 이면 할 수 있는 작업이 O(N^2)로 커진다.

그렇다면, 이를 해결하기 위한 방법으로는 성능을 무시한 방법으로는 Array의 `+`와 같은 것을 Dictionary도 만들면 좋을것이다.
```Swift
func +<Key: Hashable, Value>(lhs: [Key: Value], rhs: (Key, Value)) -> [Key: Value] {
    var result = lhs
    result[rhs.0] = rhs.1
    return result
}
```
여기서 `=`를 통해 `id->User`를 매핑하는 것을 한줄로 끝낼 수 있다.
```Swift
users.reduce([Int: User]()) { $0 + ($1.id, $1) }
```
이런 `+`와 같은 개념은 함수형 불변객체(immutable)프로그래밍적인 생각이다.

이런 불변객체은 최근 프로그래밍기법중 하나이지만, Swift언어에서는 불변객체에 집착할 필요가 없다. 이유는 Swift언어는 값형을 중심의 언어로 가변객체(mutable)값형은 불변객체 참조형에 해당하기 때문이다. 이런 값형의 특성을 사용하여 간단하게 해결되는 방법은 다음과 같다.
```Swift
users.reduce(into: [Int: User]()) { $0[$1.id] = $1 }
```
클로저식 내부에 `$0[$1.id] = $1`만으로 상당히 직관적이다. 바로 `id->User`맵핑을 해주기 때문에 가독성이 해결된다. reduce는 원래 함수형 언어에서 나온 것으로 불별객체 프로그래밍과 잘 맞는다. 지금까지의 reduce처리결과를 combine리턴값으로 리턴해야 Dictionary로 처리하는 새로운 요소를 추가할때마다 새 Dictionary인스턴스를 생성하고 다시 만들어야 한다. 새로운 reduce값의 combine리턴값으로 결과를 리턴하는 것이 아니라 combine인수에 대해 변경하여 결과를 만들어야 한다. 첫번째 인자가 참조로 전달되는 것이다.

```Swift
func reduce<T>(into initial: T, _ combine: (inout T, Element) -> ()) -> T
```
결과 T값형인 경우, inout이 아니면 첫번째 인수에 대해 변경하고 그것을 결과에 반영시킬 수 없다. combine 첫번째 인자가 inout이 되어 있는 것은 다음과 같은 코드로 구성할 수 있다.

```Swift
var result: [Int: User] = [:]
for user in users {
    result[user.id] = user
}
```
inout에서 직접 결과의 Dictionary의 변경를 추가하고 있기 때문에 성능상의 문제를 for반복문으로 만든 코드와 동일하다. 클로저를 호출하는 오버헤드도 최적화를 통해 클로저가 인라인하여 없어지는 것을 기대할 수 있다. 또한 for문을 사용하면 minimumCapacity를 지정해서 내부 버퍼낭비를 피할 수 있다.
```Swift
users.reduce(into: [Int: User](minimumCapacity: n)) { $0[$1.id] = $1 }
```

또한, 다음은 Array를 만들때 성능향상을 위해 새 reduce를 사용하는 것을 보여준다.
```Swift
[2, 3, 5].reduce(into: []) { $0.append(($0.last ?? 0) + $1) }
```
--------------------------------------------------------------------
[출처](https://swifter.kr/2017/05/11/swift-4-0의-새로운-reduce-기능/)


