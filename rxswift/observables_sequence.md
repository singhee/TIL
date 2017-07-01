# Observables also known as Sequences
Sequence는 순차적이고 반복적으로 각각의 element에 접근 가능하도록 디자인된 데이터 타입이다. 쉽게 말해서 Sequence 는 list 와 같이 반복문을 사용할 수 있는 데이터 타입을 말한다.  RxSwift에서 Observables은 Sequences이다.

Arrays, Strings 와 같은 Sequences는 RxSwift에서 Observables가 된다. Swift Sequence Protocol을 따르는 모든 Object들의 Observable을 만들 수 있다. 
```Swift
let stringSequence = Observable.just("this is string yo")
let oddSequence = Observable.from([1, 3, 5, 7, 9])
let dictSequence = Observable.from([1:"Rx",2:"Swift"])
```
이렇게 만든 Observables를 subscribe 할 수 있다.
```Swift
/*
subscribe(on: (Event<String>) -> Void)
*/
let subscription = stringSequence.subscribe { (event: Event<String>) in
  print(event)
}
// -- 출력 --
// next(this is string observable)
// completed
```

```Swift
let subscription = oddSequence.subscribe { (event: Event<Int>) in
  print(event)
}
// -- 출력 --
// next(1)
// next(3)
// next(5)
// next(7)
// next(9)
// completed
let subscription = dictSequence.subscribe { (event: Event<(key: Int, value: String)>) in
  print(event)
}
// -- 출력 --
// next((2, "Swift"))
// next((1, "Rx"))
// completed
```
Observables는 0 혹은 그보다 많은 event를 발생시킬 수 있다. RxSwift에서 Event는 enum 타입이고 3가지 상태를 가지고 있다. 정의된 Event에 대해 살펴보면 다음과 같이 나오는것을 볼 수가 있다. 

```Swift
// Event의 정의 입니다. 3가지 상태를 가지고 있어요.
enum Event<Element>  {
    case next(Element)      // next element of a sequence
    case error(Swift.Error) // sequence failed with error
    case completed          // sequence terminated successfully
}
class Observable<Element> {
    func subscribe(_ observer: Observer<Element>) -> Disposable
}

protocol ObserverType {
    func on(_ event: Event<Element>)
}
```

# Observable 만들기
Observable의 생성은 `just`, `from`, `create`등을 이용해 만들 수 있다. 
```Swift
// just
let stringSequence = Observable.just("this is string yo")

// from
let oddSequence = Observable.from([1, 3, 5, 7, 9])

// create
func myJust<E>(element: E) -> Observable<E> {
    return Observable.create { observer in
        observer.on(.next(element))
        observer.on(.completed)
        return Disposables.create()
    }
}

myJust(element: 0)
    .subscribe(onNext: { n in
      print(n)
    })

```
`create` 함수는 Swift 의 closure를 사용해 `subscribe` 메서드를 쉽게 구현할 수 있도록 한다.
