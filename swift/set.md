# Set
일련의 값들이 순서를 가지고 집합을 이루고 있을 때 사용하는 가장 흔한 자료 구조는 `배열`이다. 배열은 원소들이 연속적으로 저장된다는 점에 기반하여 임의 원소에 빠르게 액세스할 수 있는 점과 선형적인 구조에 기반한 여러 테크닉들을 적용할 수 있다는 점에서 널리 사용된다.

하지만 배열은 집합과 관련된 문제에 대해서 항상 옳은 선택이 아닐 수 있다. 예를 들어 특정한 값이 집합내에 포함되어 있는지 여부는 `contains(_:)` 를 통해서 손쉽게 알 수 있지만, 해당 메소드의 시간 복잡도는 O(n) 이다. 이외에 배열은 두 개 이상의 배열에 대해서 합집합은 원소의 중복을 감안한다면 비교적 쉽게 얻을 수 있으나, 교집합이나 차집합에 대해서는 별도의 로직을 만들어서 이를 구해야하고 이 때의 성능도 거의 항상 O(n)일 수 밖에 없다. 즉 개별 원소의 저장소로의 배열보다 집합 자체를 다뤄야 하는 상황이라면 배열보다 더 어울리는 자료구조가 있는데, 그것이 바로 `Set`이다.

## 생성
`Set`은 기본적으로 좌변의 타입이 지정되었을 때 배열 리터럴로 만들 수 있다.
```swift 
var someSet : Set<Int> = [3, 5, 1, 8, 9]
```
혹은 배열과 비슷하게 `init(arrayLiteral:)`이나 `init(_:Sequence)`를 이용하는 게 가능하다.

```
var someSet = Set<Int>(arrayLiteral: 4, 3, 2, 1 9)
```
배열은 기본적으로 원소의 순서가 없다. 하지만 Collection 프로토콜을 지원하므로 사실, Index만 알고 있으면 [i]문법으로 subscript하는 것이 가능하다.  대신에 Set에서 원소와 관련된 연산은 다음과 같은 것들이 주가 되겠다.

1. 원소 추가 (`insert(_:)`)
2. 원소 제거 (`remove(_:)`) 
3. 원소가 있는지 검사 (`contains(_:)`)

```swift
var s: Set<Int> = [1, 2, 3, 6, 9, 15]

s.insert(24)

if let r = s.remove(7) { print("removed 7") }
else { print("not found 7") }

if r.contains(9) { print("found 9") }
else { print("not found") }
```

## 집합 연산
### 포함 관계
두 집합 관계를 찾는다. 
1. 동치: `==`로 비교한다.
2. 포함관계: `isSubset(of:)`, `isSuperSet(of:)`로 포함관계를 볼 수 있다.
3. 엄격한 포함관계: `isStrictSubset(of:)`, `isStrictSuperset(of:)`를 이용해서 완전히 포함되는지 여부를 볼 수 있다.
4. `isDisjoint(with:)` 를 통해서 서로 소 집합인지를 볼 수 있다.

### 연산
다른 집합을 가져와서 교집합, 합집합, 차집합 및 서로 차집합을 구할 수 있다. 아래의 연산은 모두 새로운 복사본을 만드는 것이다.

* intersection(_:) 교집합
* union(_:) 합집합
* subtract(_:) 차집합
* symmetricDifference(_:) 대칭차집합

원본을 변경하여 연산하는 메소드는 다음과 같다.

* formUnion(_:)
* formIntersection(_:)
* subtracting(_:)
* formSymmetricDifference(_:)

```swift 
var odds = [1, 3, 5, 7, 9]
var primes = [2, 3, 5, 7, 11]

let u = odds.union(primes) // [1,2,3,5,7,9,11]
let i = odds.intersection(primes) // [3, 5, 7, 9]
let s = odds.subtract(primes) // [1]
let d = odds.formSymmetricDifference(primes) //
```