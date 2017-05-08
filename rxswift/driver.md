# Driver
`Driver`는 RxSwift에서 다른 언어의 Rx 구현체와는 다르게 유일하게 제공하는 unit이다. `Driver`는 UI layer에서 좀 더 직관적으로 사용하도록 기능을 제공한다. Observable은 상황에 따라 MainScheduler와 BackgroundScheduler를 지정해줘야 하지만, Driver는 MainScheduler에서 사용한다. 그러기 때문에 `Driver` 유닛의 특징은 다음의 내용을 충족한다.
* Can't error out
* Observe on main scheduler
* Sharing side effects (`shareReplayLatestWhileConnected`)

위의 내용은 또한 Cocoa/UIKit에서 UI 이벤트를 구성하기 위해 주의해야 할 점과 같다.

예시로, 백그라운드에서 이벤트를 발생하는 Observable이 있을 때, 이벤트가 발생하면 UILabel에 해당값을 보여주려 한다.
UI이벤트는 메인스레드에서 동작해야하며, 에러가 발생하면 처리해주는 코드도 필요하다. 위의 3가지 조건에 맞도록 Observable을 구성하면 다음과 같다.

```swift
let observable = Array(0...5).toObservable().observeOn(ConcurrentDispatchQueueScheduler(globalConcurrentQueueQOS: .Background))

observable.observeOn(MainScheduler.instance)
	.catchErrorJustReturn(-1)
	.shareReplayLatestWhileConnected()
```
위의 코드는 메인스레드에서 동작하도록 하며, 에러처리와, subscription 공유가 가능한 코드이다.
이를 driver를 사용하면 아래와 같이 만들면 된다.

```swift
observable.asDriver(onErrorJustReturn: -1).asObservable()
// driver 내부에서 shareReplayLatestWhileConnected 가 적용된다!
```

### Variable
variable 객체는 BehaviorSubject를 wrap한 클래스이며, 에러를 발생하지 않는다. BehaviorSubject 와 같이 값이 저장되어 사용되고, 이것을 UI와 연결하고 싶을때 사용하면 된다. variable 객체는 asdriver를 통해 driver unit으로 변경이 가능하다.

## 언제 Driver를 쓰는 것이 좋을까?
UI와 관련된 것에는 Observable 대신 Driver를 쓰는 것이 좋다. Observable을 쓰게 되면 Thread를 지정해줘야 하는데, 실수가 발생하여 앱 동작에 큰 위협이 될 수도 있기 때문이다.

또한 Observer에게는 어떤 Scheduler를 사용할 것인지 일일이 다 지정을 해주며, subscription 공유도 설정해줘야 한다. 그러기 때문에 UI와 관련되었을 때는 Driver를 통해 작성하는 것이 훨씬 더 간편한 것을 알 수 있다.