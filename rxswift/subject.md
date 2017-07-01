# RxSwift - Subject
ReactiveX의 `Subject`란, Cold Observable을 Hot Observable로 변경하는 효과를 얻을 수 있다. [*Hot and Cold Observable in ReactiveX*](https://github.com/singhee/TIL/blob/master/rxswift/observable.md)

`Subject`는 `Imperative eventing` 으로 어떤 이벤트를 발생 하고 싶을때, 얼마나 많은 객체에게 그 이벤트를 전달해야 할지 모를 수 있다. 이럴때 subject를 만들어서 원하는 이벤트를 subscription(observer) 의 존재 여부와 관계없이 이벤트를 발행할 수 있다. (Subject는 Observable과 Observer 둘 다 될 수 있는 특별한 형태이다. - Observables를 Subscribe할 수 있고, 다시 emit할 수도 있다.)

ReactiveX에서는 4가지 종류의 subject가 있다. RxSwift에서 Subject는 `ObservableType`이다. 
* PublishSubject
* BehaviorSubject
* ReplaySubject
* Variable

## PublishSubject
subject는 subscribe된 시점 이후부터 해당 observer에게 이벤트들을 전달한다. subscribe된 시점 이전의 이벤트는 전달하지 않는다.
`PublishSubject`는 subscribe가 시작되면 이벤트만을 emit하며, observer들은 구독시점부터 발생하는 이벤트를 받을 수 있다. subscribe 전의 이벤트는 emit하지 않는다.  subject가 error 에 의해 종료되면, 이벤트 생성 시점 이후 발생한 subscribe는 이벤트를 받지 않고 에러를 받게 된다.

```Swift
let disposeBag = DisposeBag()
let publishTest = PublishSubject<String>()

publishTest.subscribe { event in
	print(event)
}.addDisposableTo(disposeBag)

// onNext로 새로운 값을 추가할 수 있으며, 값을 추가하면 event가 발생한다.
publishTest.onNext("I")
publishTest.onNext("am")
publishTest.onNext("sanghee")
publishTest.onCompleted()
publishTest.onNext("yoon"))

// 결과
// next(I)
// next(am)
// next(sanghee)
// completed
```

## BehaviorSubject
BehaviorSubject는 PublishSubject 와 거의 동일하지만, **초기 이벤트를 가진 Subject이다.(반드시 값으로 초기화 해주어야 한다)** 이벤트를 저장해 두고 싶을때 사용하며, subscribe가 발생하면 즉시 현재 저장된 이벤트(subscribe 하기전 마지막 이벤트)를 전달하고, 이후는 publish와 동일하다. 

```Swift 
struct Person {
	let name: String
}
let disposeBag = DisposeBag()
let behaviorTest = BehaviorSubject<Person>(value: Person(name: "Sanghee"))

behaviorTest.subscribe { event in 
	print(event)
}.addDisposableTo(disposeBag)

behaviorTest.onNext(Person(name: "Tom"))
behaviorTest.onNext(Person(name: "Harry"))
behaviorTest.onCompleted()
behaviorTest.onNext(Person(name: "Jenny"))

// 결과
// next(Person(name: "Sanghee"))
// next(Person(name: "Tom"))
// next(Person(name: "Harry"))
// completed
```

## ReplaySubject
ReplaySubject는 미리 정해진 사이즈 만큼 가장 최근의 이벤트를 새로운 Subscriber에게 전달 한다. RxSwift에서는 create(bufferSize bufferSize: Int)와 createUnbounded의 생성함수를 가진다. createUnbounded는 Subject의 생성 이후 발생하는 모든 이벤트를 저장한다.

```Swift 
struct Person {
	let name: String
}
let disposeBag = DisposeBag()
let replayTest = ReplaySubject<Person>.create(bufferSize: 2)

replayTest.subscribe { event in 
	print("subscribe 1: \(event)")
}.addDisposableTo(disposeBag)

replayTest.on(.next(Person(name: "Tom")))
replayTest.on(.next(Person(name: "Harry")))
replayTest.on(.next(Person(name: "Jenny")))

replayTest.subscribe { event in
	print("subscribe 2: \(event)")
}.addDisposableTo(disposeBag)

// 결과
// subscribe 1: next(Person(name: "Tom"))
// subscribe 1: next(Person(name: "Harry"))
// subscribe 1: next(Person(name: "Jenny"))

// subscribe 2: next(Person(name: "Harry"))
// subscribe 2: next(Person(name: "Jenny"))
```
subscribe 2 가 생긴뒤 버퍼 사이즈 (2)만큼 이전의 이벤트가 전달된 것을 확인 할 수 있다. 
만약 위의 코드에서 `ReplaySubject<Person>.create(bufferSize: 2)`를 `ReplaySubject<Person>.createUnbounded()`로 바꾸면 다음과 같이 subscribe 2 이전의 모든 이벤트가 전달된 결과가 나온다.

```Swift
// 결과
// subscribe 1: next(Person(name: "Tom"))
// subscribe 1: next(Person(name: "Harry"))
// subscribe 1: next(Person(name: "Jenny"))

// subscribe 2: next(Person(name: "Tom"))
// subscribe 2: next(Person(name: "Harry"))
// subscribe 2: next(Person(name: "Jenny"))
```

## Variable
`Variable` 은 `PublishSubject`의 Wrapper 함수이다. `PublishSubject`처럼 작동하며 더 익숙한 이름으로 사용하기 위해 만들어 졌다.
--------------------------------------------------------------
[https://brunch.co.kr/@tilltue/4](https://brunch.co.kr/@tilltue/4)
[https://medium.com/@trilliwon/rxswift-기본-익히기](https://medium.com/@trilliwon/rxswift-기본-익히기-1-d4d77ce63ca8)

