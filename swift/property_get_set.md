# 프로퍼티의 Getter, Setter 접근 권한 설정
클래스나 구조체의 프로퍼티를 읽기 전용으로 만들려면 가장 쉽게 생각 할 수 있는 것은 아래와 같이 `private` 변수를 정의하고 읽기 전용 함수를 정의하는 방법일 것이다.

```swift
struct Counter {
    private var count: Int
    
    func getCount() -> Int {
        return count
    }
}

let c = Counter(count:10)

print("count => \(c.getCount())") // 출력: count => 10
```

하지만 어쩐지 코드가 스위프트적이지 않다. 스위프트에서 프로퍼티를 선언할 때는 아래처럼 게터(getter)와 세터(setter)의 접근속성을 각각 설정할 수 있다.

```swift
struct Counter {
    public private(set) var count: Int
}

let c = Counter(count:10)

print("count => \(c.count)") // 출력: count => 10
```

`public private(set) count`의 용법을 주의깊게 보면 count 프로퍼티를 기본적으로 `public`으로 설정하면서 세터(setter)에 대해서는 `private`으로 설정한 것을 알 수 있다. 이런 코드가 좀 더 스위프트적이라고 할 수 있다.

