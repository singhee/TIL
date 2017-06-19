# RxSwift 예제로 감잡기 : RxSwift 시작을 위한 간단한 예제들 
[출처](https://news.realm.io/kr/news/how-to-use-rxswift-with-simple-examples-ios-techtalk/)

## UI Event
```Swift
func reload() {}

func setup() {
	reloadButton.rx.tap 	// Observable<Void>
		.subscribe(onNext: { [weak self] in self?.reload() })
		.addDisposableTo(disposeBag)
}
```
옵저버블에서 나오는 이벤트를 비동기식으로 받아서 로직을 붙이는 것으로 많이 사용하는데, 앱 개발을 할 때 가장 많이 사용하는 것이 UI Event 이다. 버튼이 탭 이벤트를 받을 때마다 탭이 됐다는 void 이벤트가 발생하고, 뒤에서 이를 구독해서 reload라는 로직을 붙여서 구성한 예제이다. 이런 식으로 간단하게 이용하는 것이 Rx의 기본적인 패턴이다.

```Swift
func reload() {}

func setup() {
	reloadButton.rx.tap 	// Observable<Void>
		.bindNext(reload)
		.addDisposableTo(disposeBag)
}
```
이렇게 구독을 하면 탭을 할 때마다 onNext가 불리게 되는데, RxSwift에서는 bindNext() 라는 간결한 문법을 제공하고 있다. reload 함수에 로직을 넣으면 된다. **`bindNext()`도 self 레퍼런스를 strong하게 capture하기 때문에 사용하지 않게 될 때 명시적으로 disposeBag을 해제하는 등의 주의를 기울여야 한다.**

```Swift
func reload() {}

func setup() {
	reloadButton.rx.tap 	// Observable<Void>
		.do(onNext: {
			print("Reload Button Clicked.")
			Analytics.buttonReload.send()
		})
		.bindNext(reload)
		.addDisposableTo(disposeBag)
}
```
또한 메서드 체이닝으로 오퍼레이터를 추가할 수 있는데, 위 코드처럼 중간에 이벤트에 대한 로그를 넣을 수도 있다.

```Swift
func reload() {}

func setup() {
	reloadButton.rx.tap 	// Observable<Void>
		.debounce(0.3, scheduler: MainScheduler.instance)
		.do(onNext: {
			print("Reload Button Clicked.")
			Analytics.buttonReload.send()
		})
		.bindNext(reload)
		.addDisposableTo(disposeBag)
}
```
탭을 리로드할 때 너무 여러번 불리는 것을 막기 위해 debounce 함수로 일정 시간 안에 불리는 것은 무시할 수도 있다. [RxMarbles](http://rxmarbles.com/#debounce)라는 사이트에서 오퍼레이터의 결과가 실제로 어떻게 나오는지 인터렉티브하게 볼 수 있으니 참고하면된다.

## REST API
UI 말고도 앱에서는 네트워크 이벤트를 많이 사용하는데, 동기적으로 사용하면 UI가 블럭되거나, 스레드에서 처리하더라도 UI로 넘어갈 때 괴로움을 많이 겪게 된다. 이런 어려움도 RxSwift로 해결할 수 있다.

```Swift
func reload() {
	API.default.request(.getPage)			// Observable<JSON>
		.map { json in Page(json: json) }	// Observable<Page>
		.observeOn(MainScheduler.instance)
		.subscribe(onNext: { [weak self] page in
			self?.display(page: page)
		})
		.addDisposableTo(reloadDisposeBag)
}

func display(page: Page) { }
``` 
위의 예제는 getPage라는 API 요청을 보내면 JSON 형태의 옵저버블이 나오는 예제이다. map으로 JSON을 Page로 변환하는 체이닝 메서드를 붙이고, 구독을 해서 Page를 이용해서 디스플레이를 하는 로직을 붙일 수 있다.

이런 API가 언제 호출될지가 궁금할 것이다. hot, cold의 두 가지 타입을 만들 수 있다. 예제의 경우 보통 cold 형태의 옵저버블로 만들어서 구독이 실행될 때 내부의 함수가 실행되고 결과가 비동기적으로 나와서 아래 부분이 실행되게 된다. reload 함수가 불렸을 때는 API 함수 자체는 그냥 넘어가고, 내부 블럭에서 다른 스레드로 만들었다면 subscribe가 불렸을 때 실제 API 호출이 넘어가서 불리게 된다.

```Swift
func reload() {
	API.default.request(.getPage)			// Observable<JSON>
		.map { json in Page(json: json) }	// Observable<Page>
		.observeOn(MainScheduler.instance)
		.bindNext(display)
		.addDisposableTo(reloadDisposeBag)
}

func display(page: Page) { }
```
역시 간결한 문법을 제공하므로 같은 코드를 위 예제처럼 축약해서 사용할 수 있다.

```Swift
func reload() {
	API.default.request(.getPage)			// Observable<JSON>
		.map { Page(json: $0) }				// Observable<Page>
		.observeOn(MainScheduler.instance)
		.bindNext(display)
		.addDisposableTo(reloadDisposeBag)
}

func display(page: Page) { }
```
RxSwift 문법은 아니지만 Swift의 문법을 사용해서 다시 보다 간결하게 바꿀 수 있다.

```Swift
func reload() {
	API.default.request(.getPage)			// Observable<JSON>
		.retry(2)
		.map { Page(json: $0) }				// Observable<Page>
		.observeOn(MainScheduler.instance)
		.bindNext(display)
		.addDisposableTo(reloadDisposeBag)
}

func display(page: Page) { }
```
API를 호출했을때 에러가 나면 다시 호출하고, 몇 번 에러가 났는지 카운트해야 하지만, RxSwift를 사용하면 이 역시 간단하게 구현할 수 있다.

```Swift
func reload() {
	API.default.request(.getPage)			// Observable<JSON>
		.subscribeOn(SerialDispatchQueueScheduler(qos: .background))
		.retry(2)
		.observeOn(ConcurrentDispatchQueueScheduler(qos: .background))
		.map(Page.init)						// Observable<Page>
		.observeOn(MainScheduler.instance)
		.do(
			onNext: { print("Reload success: \($0)") },
			onError: { error in print("Reload failed! \(error)") },
			onCompleted: { print("Reload completed") }
		)
		.bindNext(display)
		.addDisposableTo(reloadDisposeBag)
}
```

스레드도 쉽게 넘나들 수 있는데, subscribeOn으로 옵저버블이 생성되고 결과가 나오는 것을 어느 스레드에서 사용할지 지정할 수 있고, 나온 결과는 observeOn에서 하단 부분에 대해 적용할 수도 있다. 예를 들어 Page.init에서 JSON 파싱이 오래 걸리는 경우라면 백그라운드로 넘겨서 파싱을 한 후 UI 스레드로 넘겨서 결과를 업데이트할 수 있다.

그 밖에도 앞서 말씀드린대로 로그를 쉽게 추가할 수도 있다.









