# try
스위프트에서의 에러 처리에 관한 내용에 대해 다루어보자. 스위프트에서는 에러 처리를 엄격하게 다룬다. 문법적으로도 서술적으로, 에러를 던질 가능성이 있는 메서드를 호출할 때 `try`키워드를 사용해야 하며, 에러를 받아서 처리하는 코드는 `do-catch`문으로 감싸야 한다. 

```swift
do {  
    // URL 상의 내용으로 NSData를 생성
    let data = try NSData(contentsOfURL: URL, options: [])

    ...

} catch {
    // 에러 처리
    ...
}
```

`do`구절 내에서 에러가 발생하면 `catch`구절 내에서 에러를 처리하게 된다. 이러한 방식이 스위프트의 에러 처리에 대한 기본 방식이다.

## try, try?, try!
스위프트에서는 세 가지 `try` 용법을 정의하였는데, 기본적인 `try`의 활용은 위에서의 `do-catch` 처럼 사용하는 것이다. 나머지 두 가지 방식은 `try?` 와 `try!`를 사용하는 방식이다.

### try?
`try?` 키워드를 사용하면 함수에서 리턴한 값이 옵셔널로 변경되는데, 에러가 발생하면 옵셔널로 `nil`을 리턴하고 성공한 경우에는 값이 들어 있는 옵셔널을 리턴한다.

`try?` 는 보통 옵셔널 바인딩과 함께 사용한다. 아래처럼 생성자 `init(contentsOfURL:options:)`를 호출하면서 `do-catch`문을 사용하지 않는 대신, 성공인 경우 리턴된 옵셔널에 값이 들어있게 된다.
```swift
if let data = try? NSData(contentsOfURL: URL, options: []) {  
    print("Data Loaded")
} else {
    print("Unable to Load Data")
}
```

또한 `try?` 키워드는 위와 같은 원리로 `guard`문과 같이 사용할 수 있다.
```swift
func loadContentsOfFileAtURL(URL: NSURL) -> String? {  
    guard let data = try? NSData(contentsOfURL: URL, options: []) else {
        return nil
    }

    return String(data: data, encoding: NSUTF8StringEncoding)
}
```

### try!
`try!` 를 사용하는 것은 좀 더 위험하다. `try` 키워드에 느낌표를 붙이면 `error propagation`를 비활성화 시킨다. 에러가 던져지면, 어플리케이션은 런타임 에러가 발생하면서 강제 종료된다는 뜻이다. 따라서 `try!` 는 가능한 사용하지 않는것이 좋다. 

따라서, `try!`는 옴셔널이 절대로 `nil`이 될 수 없는 경우에 강제 언랩핑을 하는 것처럼 절대로 실패하거나 오류가 발생할 가능성이 없는 경우에만 사용헤야 한다. 


[출처](http://bartjacobs.com/error-handling-in-swift-with-the-try-keyword/)