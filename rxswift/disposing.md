# Disposing
Observables의 사용이 끝나면 메모리를 해제해야 한다. 그 때 사용할 수 있는것이 Dispose이다. RxSwift에서는 `DisposeBag`을 사용하는데 DisposeBag instance의 `deinit()` 이 실행 될 때 모든 메모리를 해제합니다.
```Swift
let disposeBag = DisposeBag()
let stringSequence = Observable.just("RxSwift Observable")
let subscription = stringSequence.subscribe { (event) in
  print(event)
}

// subscription 을 disposeBag에 넣어 메모리를 해제한다.
subscription.addDisposableTo(disposeBag)

// 빠르게 비워주고 싶을때는 disposeBag을 새로 만들면 된다.
self.disposeBag = DisposeBag()
```

`subscription.dispose()`를 통해 직접 `dispose()`를 호출 할 수 있다. 
하지만 직접 호출하는 것은 좋은 코드가 아니다. Thread가 다를 때 Observable을 사용하기도 전에 메모리를 비워주는 일이 발생할 수 있기 때문이다. 메모리를 해제하는 다른 방법으로 Take Until 이 있다. 사용방법은 다음과 같이 사용하면 된다.

```Swift
sequence
    .takeUntil(self.rx.deallocated)
    .subscribe {
        print($0)
    }
```