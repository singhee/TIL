# Functional Reactive Programming with RxSwift 
[realm - functional-reactive-rxswift](http://news.realm.io/news/slug-max-alexander-functional-reactive-rxswift/)

## Introduction
최근 Rx에 대한 관심이 많아졌다. Rx는 `Observable<Element>`인터페이스로 표현되는 포괄적인 추상개념이며, RxSwift는 Rx의 Swift 버전이다.

다음으로 나오는 코드를 보자. 어쩌다가 다음과 같이 복잡한 모양이 됐을까?
```Swift 
Alamofire.request(.POST, "login", parameters: ["username": "max", "password": "insanity"])
  .responseJSON(completionHandler: { (firedResponse) -> Void in
    Alamofire.request(.GET, "myUserInfo" + firedResponse.result.value)
      .responseJSON(completionHandler: { myUserInfoResponse in
        Alamofire.request(.GET, "friendList" + myUserInfoResponse.result.value)
          .responseJSON(completionHandler: { friendListResponse in
            Alamofire.request(.GET, "blockedUsers" + friendListResponse.result.value)
              .responseJSON(completionHandler: {

              })
            })
          })
    Alamofire.request(.GET, "myUserAcccount" + firedResponse.result.value)
      .responseJSON(completionHandler: {
      })
    })
```
이는 Alamofire 요청을 위한 코드이다. 잘못된 점이 보이는가? Alamofire는 `HTTP`요청을 하는 코드이다. 그런데 위처럼 블록 기반으로 얽혀서 계속 indent가 증가하는 코드는 그 결과를 예측할 수 없다는 문제가 있다. 네트워크 통신이 실패할 경우는 어떨까? 에러 핸들러가 있지만, 예외를 어디서 처리해야 할지 알 수 없다. Rx는 이러한 상황을 해결해 준다.

## Back to the Basics
새로운 이벤트가 있으면 collection도 있다. [1, 2, 3, 4, 5, 6] 과 같은 배열 혹은 리스트의 모양이다. Swift 라면 `filter`를 활용해 짝수만 거를 수 있다.
```Swift
[1, 2, 3, 4, 5, 6].filter{ $0 % 2 == 0 }
```

5를 곱한 후 다시 배열로 만들려면 어떻게 할까?
```Swift
[1, 2, 3, 4, 5, 6].map{ $0 * 5 }
```

전체 합은 어떻게 구할까?
```
[1, 2, 3, 4, 5, 6].reduce(0, +)
```

의미가 잘 나타나는 코드이다. for loop를 돌리지 않고 중간 상태를 저장하지 않아도 된다. 사용자들이 이미지를 다운받거나 네트워크 통신을 하고 친구를 추가하는 활동을 좋아하므로 인터넷을 필수이고, IO를 아주 많이 사용하게 된다. 따라서 기기 메모리를 넘어선 영역의 요소들과 소통할 수 있어야 한다. 이런 비동기적인 활동은 실패할 수 있고, 문제가 발생할 가능성도 높다.

## The Rx Bill of Rights 
> We the people have the right to manipulate async events just like iterable collections.
> 우리는 비동기 이벤트를 마치 **iterable 컬렉션**처럼 간단하게 다룰 권리가 있다.

## Observables
Rx 에서 `Observables`를 생각해 보자. `Observables`는 안전한 형변환이 가능한 이벤트로 다른 종류의 데이터를 넣고 뺄 수 있다. RxSwift 는 쉽게 적용할 수 있다.

```
pod 'RxSwift', '~> 2.0.0-beta.3'
import RxSwift
```

`just` 를 사용하면 간단하게 `Observable`을 생성할 수 있다. 어떤 타입의 변수를 넣어도 해당 타입의 `Observable`을 반환해준다.
```Swift 
just(1)  //Observable<Int>
```
배열을 넣고 이벤트를 하나씩 꺼내려면 어떻게 해야할까?
```Swift
[1,2,3,4,5,6].toObservable()  //Observable<Int>
```
`Observable<Int>`타입을 반환할 수 있다.



만약 로컬 데이터베이스에 데이터를 저장하는 API가 있다면 다음과 같은 모습일 것이다.
```Swift 
create { (observer: AnyObserver<AuthResponse>) -> Disposable in


  return AnonymousDisposable {

  }
}
```

`create`를 호출하면 블록을 돌려준다. 이 블록이 Observable를 만들어주는데, 이는 누군가 이 이벤트를 구독할 것이라는 뜻이다. 당장은 AnonymousDisposable을 무시해도 된다. 그 다음 줄이 Observable를 만들도록 API를 처리할 곳이다.

Alamofire와 비슷한 코드를 만들면 다음과 같다.
```Swift 
create { (observer: AnyObserver<AuthResponse>) -> Disposable in

  let request = MyAPI.get(url, ( (result, error) -> {
    if let err = error {
      observer.onError(err);
    }
    else if let authResponse = result {
      observer.onNext(authResponse);
      observer.onComplete();
    }
  })
  return AnonymousDisposable {
    request.cancel()
  }
}
```
이벤트의 결과와 에러 값을 담은 콜백을 받는다. 다른 클라이언트 SDK의 코드이기 때문에 API를 고칠 수는 없지만, 반환값을 `Observable`로 바꿀 수는 있다. 에러가 있다면 `observer.onError()`를 부르는데, 이는 구독하고 있는 상대방에게 뭔가 실패했음을 알려주는 것이다. 실제 응답을 받으면 `observable.onNext()`를 부른다. 다음으로 뭔가 끝났다면 `onComplete()`를 부른다. 
`AnonymousDisposable`는 뷰 컨트롤러를 떠났거나, 서비스를 종료해서 더이상 해당 요청을 할 필요가 없을 경우처럼, 어떤 인터럽트를 받았을때 부르는 액션이다. 비디오 업로드처럼 큰 작업을 할 때 유용하다. `request.cancel()`을 하면 모든 작업을 끝냈을 때 할당됐던 자원을 제거할 수 있다.

## Listening to observables
observable의 구독은 아래처럼 배열을 만든 후, 여러 다른 객체들을 부를 수 있는 확장 함수인 toObservable()를 부른다. 리스너 함수를 벌써 완성한것이다.
```Swift 
[1,2,3,4,5,6]
  .toObservable()
  .subscribeNext {
    print($0)
  }
```
iterable 처럼 구독 리스너 이벤트는 'onError' 상황이나 `onNext`, 혹은 `onCompleted`상황에서 여러 다른 정보를 줄 수 있습니다. 필요할 때 구독하기만 하면 됩니다.
```Swift 
[1,2,3,4,5,6]
  .toObservable()
  .subscribe(onNext: { (intValue) -> Void in
    // Pumped out an int
  }, onError: { (error) -> Void in
    // ERROR!
  }, onCompleted: { () -> Void in
    // There are no more signals
  }) { () -> Void in
    // We disposed this subscription
  }
```

## Combining Observables
Rx를 사용한다는 것은 소켓 서비스를 사용하는 것과 비슷하다. 현제 계좌 잔고를 보여주는 UI가 있고 주식 시세 표시기를 구독하는 웹소켓 서비스를 생각해보도록 하자. 주식 시세 표시기가 여러 다른 이벤트를 보여주면 사용자가 매입할 수 있는지 표시해준다. 계좌 잔고가 낮으면 구매 버튼을 비활성화하고, 매입자의 가격 범위에 들어오면 활성화하는 기능이다.

```Swift 
func rx_canBuy() -> Observable<Bool> {
  let stockPulse : [Observable<StockPulse>]
  let accountBalance : Observable<Double>

  return combineLatest(stockPulse, accountBalance,
    resultSelector: { (pulse, bal) -> Bool in
    return pulse.price < bal
  })
}
```

`combineLatest`는 두 개의 최근 이벤트를 결합한다. 계좌 잔고 보다 주식 시세 표시기의 가격이 낮은지에 따라 블록이 작용한다. 즉, 해당 주식을 살 수 있다는 뜻이다. 이를 활용해 두 observable을 결합하고 조건을 충족하는지 판정하는 로직을 세울 수 있다. 결과로는 `Bool` 타입의  observable 값을 반환합니다.

```Swift 
rx_canBuy()
  .subscribeNext { (canBuy) -> Void in
    self.buyButton.enabled = canBuy
  }
```
rx_canBuy 메서드는 구독한 상대방에게 boolean값을 준다. 따라서 self.buyButton 값은 canBuy 값과 같다.












