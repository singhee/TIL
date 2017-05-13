# Observable
## Hot and Cold Observables
[ReactiveX 공식 사이트 - Observable](http://reactivex.io/documentation/observable.html)
> When does an Observable begin emitting its sequence of items? It depends on the Observable. A “hot” Observable may begin emitting items as soon as it is created, and so any observer who later subscribes to that Observable may start observing the sequence somewhere in the middle. A “cold” Observable, on the other hand, waits until an observer subscribes to it before it begins to emit items, and so such an observer is guaranteed to see the whole sequence from the beginning.

‘언제 아이템들이 Emit 되는가’ 는 Observable 에 따라 다르다. “Hot” 이면 만들어 졌을때 emit 되기 시작하고, 나중에 구독하는 Observer 는 중간 아무데서나 시퀀스를 관찰한다. 
“Cold” 는 반대로 Observer 가 구독을 시작할때까지 기다리고, 구독을 시작할때 아이템들을 emit 하기 시작합니다. 따라서 Observer 가 시작부터 시퀀스 전체를 관찰하는 것을 보장받는다.

> In some implementations of ReactiveX, there is also something called a “Connectable” Observable. Such an Observable does not begin emitting items until its Connect method is called, whether or not any observers have subscribed to it.

ReactiveX 에는 “Connectable” Observable 이 있다. observer 가 구독을 하던말던 상관없이 connect 함수가 불리기 전엔 아이템을 emit 하지 않는다.

보통 `ConnectableObservable` 을 `Hot Observable` 라 부르고, 일반적으로 많이 쓰는 Observable 이 `Cold Observable`이라 불린다. 그리고 Observable 을 확장한 것이 `ConnectableObservable` 이다. `ConnectableObservable`는 Observable 의 `publish()` 함수를 이용해서 을 만들수 있다. 

Observable은 자동적으로 emit 되고, ConnectableObservable 은 수동적으로 emit 된다. 즉 ConnectableObservable 은 트리거( connect 함수 )를 호출해야 emit 이 되기 시작 한다. 그러면 위에서 정의했던 Hot 와 Cold 의 의미와는 대응이되지 않는듯 하다. Hot / Cold 와 ConnectableObservable / Observable 을 따로 보는게 맞는듯 하다.

## Hot vs Cold Observables
Hot Observables | Cold Observables 
----------------|-------------------
시퀀스 | 시퀀스
Observer가 구독하든 말든 상관없이 아이템을 발행 | Observer가 구독해야지 아이템을 발행
Variables / properties / constants, tap coordinates, mouse coordinates, UI control values, current time | Async operations, HTTP Connections, TCP connections, streams
Usually contains ~ N elements | Usually contains ~ 1 element
시퀀스 계산 리소스는 구독하고 있는 모든 Observer 사이에 공유됨 | 시퀀스 계산 리소스는 구독하고 있는 Observer마다 할당됨
Usually stateful (상태유지) | Usually stateless (상태유지 X)

## ConnectableObservable
### `publish()`
Observable 객체의 `publish()` 함수를 호출해주면 ConnectableObservable 를 반환한다. `publish()`는 operator의 한 종류로 보면 된다. ConnectableObservable 의 큰 특징은 subscribe 해주는것과 상관없이 connect 를 해주어야 item 을 emit 하기 시작한다. ( 그냥 Observable의 경우는 누군가가 subscribe 를 하는 순간부터 emit 을 한다. ) 그리고 emit 된 item 을 소비하는 구독자들은 여러개가 붙어 있더라도 한번의 연산만하고 거의 동시에 같은 값을 소비하게 된다. 따라서 ConnentableObservable은 connect 의 시점이 매우 중요하다고 볼 수 있다.

### `refCount()`
> make a Connectable Observable behave like an ordinary Observable

`refCount()`는 ConnectableObservable의 함수로써, ConnectableObservable 을 Observable 로 바꿔주는 역활을 한다. ConnectableObservable 은 connect 를 해주어야 emit 을 시작 하지만, refCount 를 통해 만들어진 Observable 은 기존 Observable 처럼 누군가 subscribe 시작할때 connect 되어 emit 을 시작한다. 그리고 모든 구독자가 unsubscribe 하면 Observable 도 unsubscribe 된다.



## Observable 생성
### `Observable.create`
`Observable.create`은 Observable 의 가장 기본적인 생성메서드이다. Observable은 다음과 같이 생성할 수 있다. 

```swift 
let createTest = Observable<String>.create { observer -> Disposable in 
	observer.on(.next("next"))
	observer.on(.completed)
	return Disposables.create {
		print("dispose")
	}
}
```
subscribe에 대한 문법은 다음과 같다. 
```swift 
// createTest.subscribe(onNext: ((String) -> Void)?, onError: ((Error) -> Void)?, onCompleted: (() -> Void)?, onDisposed: (() -> Void)?)

createText.subscribe(onNext: { event in
	print(event)
}).addDisposableTo(disposeBag)

```
1. create는 원하는 type으로 이벤트를 발생시키는 Observable을 생성한다. 
2. Observable은 subscribe 되면 이벤트를 발생하기 시작한다. 그리고, onNext, onError, onComplete 를 전달한다.
3. dispose는 subscribe에서 명시적으로 호출하거나 전달한 disposeBag을 초기화 하거나, onComplete가 호출 된 뒤에 dispose된다.
4. on(.error(error)) 와 on(completed) 는 같은 스택에서 부르지 않는다.

### `Observable.generate` 
`Observable.generate`는 조건식을 가진 생성이다. 
```swift
// Observable.generate(initialState: Element, condition: (Element) throws -> Bool, iterate: (Element) throws -> Element) 

let generateTest = Observable.generate(initialState: 1, condition: {$0 < 30}, iterate: {$0 + 10})
```

### `Observable.just`
`Observable.just`는 단일 이벤트를 발생하는 Observable을 생성한다.
```swift
let justText = Observable<String>.just("just one")
justTest.subscribe { event in 
	print(event)
}.addDisposableTo(disposeBag)
```
1. 초기값을 가지고 첫 emit를 발생시킨다.
2. 조건식을 통해 Observable의 종료를 결정한다.
3. 초기값을 기반으로 다음 이벤트를 가공하는 함수를 가진다.
4. 네 번째 파라미터로 scheduler를 전달할 수 있는데 기본적으로는 현재 thread를 사용한다.
(scheduler: ImmediateSchedulerType = CurrentThreadScheduler.instance)
별도의 thread에서 동작하도록 하고 싶다면 scheduler를 전달.




