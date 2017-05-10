# flatMap
`flatMap` 은 `flatten`과 `map`의 합성이다. Array 타입에는 `flatten` 메소드가 정의되어 있는데, 이는 배열이나 Range를 원소로 갖는 배열(2차 배열)을 1차 배열로 풀어주는 역할을 한다. 

```swift 
let a = [[1,2,3], [4,5,6], [7,8,9]]
let b = a.flatten() // [1,2,3,4,5,6,7,8,9]
```
따라서 `flatMap`은 `(T) -> [U]` 타입의 transform 함수로 맵핑한 결과를 flatten하여 리턴하는 함수로 이해할 수 있다. 

`flatMap`의 타입 시그니처는 다음과 같이 정의되어 있다.
```swift 
func flatMap<SegmentOfResult: Sequence> 
    (_ transform: @noescape (Element) throws -> SegmentOfResult) 
    rethrows -> [SegmentOfResult.Iterator.Element]
```

이는 배열 내 원소들이 어떤 컨테이너에 싸여 있을 때, 그 컨테이너를 벗겨낸 값들로 맵핑되는 동작이라고 봐야 한다. 이 관점에서 본다면 `flatMap`은 배열 뿐만 아니라 옵셔널타입에 대해서도 적용될 수 있다. 다음 코드는 옵셔널 값에 대해서 `(T) -> U?` 타입의 클로저를 맵핑했을 때의 `map` 과 `flatMap`의 차이를 보여준다.

```swift
var a: Int? = 4 // Optional(4)
let q: (Int) -> Int? = { n in
    if n % 2 == 0 {
        return n + 3
    }
    return nil
}

let b = a.map(q)
print(b)
/// Optional(Optional(5))

let c = a.flatMap(q)
print(c)
/// Optional(5)
```
옵셔널 내에 다시 옵셔널로 둘러싸진 결과를 내놓는 맵핑에 대해서 `flatMap`은 하위 옵셔널을 풀어낸 값이 나올 수 있게 하는 것이다. 

물론 위에서 말한 컨테이너 속의 컨테이너라는 개념이 같은 컨테이너에 대해서만 적용되는 것은 아니다. 옵셔널의 배열에 대해서도 얼마든지 적용이 가능하다.
```swift
let a = [1,2,3,4,5]
let c : (Int) -> Int? = { n in 
    if n % 2 == 0 {
        return n * 2 
    }
    return nil
}

let b = a.flatMap(c)
print(b)
/// [4, 8]
```
결국 `flatMap`은 컨테이너의 깊이를 증가시키지 않고 map한 결과를 얻는 함수라고 이해하면 된다. 특히 위에서 살펴본바대로 T -> [U] 혹은 T -> U? 타입의 함수를 맵핑할 때 매우 유용하다.

예를 들어 일련의 숫자의 집합을 숫자값의 집합으로 바꾸는 과정을 생각해보자. 문자열을 정수값으로 변환하는 경우 `Int.init?` 이 적용되기 때문에 변환한 값은 정수가 아닌 `Int?` 타입이 된다. 따라서 보통은 다음과 같은 코드를 작성하게 된다.
```swift 
let words = ["123", "456.7", "eighty nine", "10", "100"]
let numbers = words.map{ Int($0) }.filter{ $0 != nil }.map{ $0! }
```

하지만, `flatMap`을 적용하면 다음과 같이 보다 간단하게 코드를 작성할 수 있다.
```swift 
let numbers = words.flatMap{ Int($0) }
```




-----------------------------------------------------------------
[http://soooprmx.com/wp/archives/6784](http://soooprmx.com/wp/archives/6784)

