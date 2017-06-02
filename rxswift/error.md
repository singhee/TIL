# RxSwift Erro처리
에러발생을 감지하는 `catchError` 메서드와 다시 정상적인 이벤트를 받기 위한 `retry`에 대해 알아보자. 

## `catchError`, `catchErrorJustReturn`
`catchError`와 `catchErrorJustReturn`는 에러를 감지했을때 `onError`를 부르지 않도록 하고, 이벤트를 발생해서 시퀀스를 진행하고 `onComplete`될수있도록 한다. 

```Swift 
let zoo = Observable<String>.create { observer in
	for count in 1...3 {
		observer.on(.next("Rabbit\(count)"))
	}

	let error = NSError(domain: "dummyError", code: 0, userInfo: nil)
	observer.on(.error(error))
	return Disposables.create {
		print("dispose")
	}
}

zoo.catchError { error -> Observable<String> in 
	return Observable.just("Zoo Closed")
}.subscribe(onNext: { test in 
	print(test)
}).addDisposableTo(disposeBag)

```
위의 코드는 "Rabbit" 이벤트를 3번 발생하고 에러를 전달하는 observable을 생성한다.
catchError에서 에러를 받았지만, 단일 이벤트 observable 연산자인 `just`를 통해 "Zoo Closed"이벤트를 전달하도록 한다.

결과는 다음과 같다.
```

Rabbit1
Rabbit2
Rabbit3
Zoo Closed
dispose
```

### `catchError` 여러 이벤트 전달
`catchError`에서는 여러개의 이벤트를 보내줄 수도 있다. 
`return Observable.of("Fox1", "Fox2", "Closed Zoo")

### `catchErrorJustReturn`
`catchErrorJustReturn`은 위와 같이 error가 발생했을때 설정해둔 단일 이벤트를 전달하도록 하는 연산자이다. subscribe에서 에러를 감지하는 것이 아닌, Observable에서 에러에 대한 기본 이벤트를 설정하는 부분에 주목하자.

```Swift
let zoo = Observable<String>.create { observer in
	for count in 1...3 {
		observer.on(.next("Rabbit\(count)"))
	}
	let error = NSError(domain: "dummyError", code: 0, userInfo: nil)
	observer.on(.error(error))
	return Disposables.create {
		print("dispose")
	}
}.catchErrorJustReturn("Rabbit End")

zoo.subscribe(onNext: { test in
	print(test)
}).addDisposablesTo(disposeBag)

```
결과는 앞서 제시한 코드 결과와 같다. 

## `Retry`
`Retry`는 시퀀스가 정상동작 하기를 기대하며 재시도 하는 연산자이다.

```Swift
var errorCount = 0
let zoo = Observable<String>.create { observer in
	for count in 1...3 {
		if errorCount == 2 {
			print("error")
			errorCount += 1
			let error = NSError(domain: "dummyError", code: 0, userInfo: nil)
			observer.on(.error(error))
		}else {
			errorCount += 1
			observer.on(.next("Rabbit\(count)"))
		}
	}

	observer.on(.completed)
	return AnonymousDisposable {
		print("dispose")
	}
}

zoo.retry().subscribe(onNext: { test in
	print(test)
}).addDisposableTo(disposeBag)

```
첫번째 두번째 이벤트 이후에만 에러를 한번 발생하는 Observable 을 생성한다. 이후 `retry()`를 통해 Observable이 다시 수행되고, 두번째는 모든 이벤트를 정상적으로 발행한다.

결과는 다음과 같다. 
```
Rabbit1
Rabbit2
error
dispose
Rabbit1
Rabbit2
Rabbit3
dispose
```

### retry(maxAttemptCount: Int)
`retry(maxAttemptCount: Int)`는 retry를 시도할 최대 count를 지정할 수 있다. 


### retryWhen
retry하는 시점을 지정할 수 있다. 
첫번째 에러가 발생했을때 지연시간을 가질 Observable을 생성하며, 이 Observable의 이벤트가 발생 했을때 다시 subscribe를 수행해서 본래 Observable의 시퀀스가 수행되어 이벤트를 받을 수 있다. 

바로 위의 Retry 예제중 subscribe부분을 아래와 같이 바꾸면 결과는 다음과 같을 것이다. 
```Swift
zoo.retryWhen({(_) -> Observable<Int> in
	return Observable.timer(3, scheduler: MainScheduler.asyncInstance)
}).subscribe(onNext: { test in
	print(test)
}).addDisposableTo(disposeBag)
```
 
```
// 결과 
Rabbit1
Rabbit2
error
dispose
(3초뒤)
Rabbit1
Rabbit2
Rabbit3
dispose
```
















