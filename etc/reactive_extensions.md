# Reactive Extensions - Rx
`Reactive Extensios` 즉, `Rx`란 비동기와 이벤트 기반 프로그램을 작성하는 데 필요한 **Ovservable** 시퀀스, LINQ형식의 쿼리문 처리에 대한 기능으로 구성된 라이브러리를 말한다. 

Data sequences는 파일, 웹 서비스, 웹 서비스 요청, 시스템 알림 또는 유저 인풋같은 일련의 이벤트들 처럼 많은 형식을 취할 수 있다. 

> ReactiveX is a library for composing asynchronous and event-based programs by using observable sequences. 
> [ReactiveX 홈페이지 정의](http://reactivex.io/intro.html)


## Rx를 지탱하는 세 가지 키워드 
MS 에서는 Rx를 

### 1. Reactive Programming(RP)
![Figure1. Stream](../images/stream.png)
> **Reactive programming** is programming with asynchronous data streams.
You can listen to that stream and react accordingly.

* Reactive Programming은 기본적으로 모든 것을 스트림(stream)으로 본다. 이벤트, ajax call 등 모든 데이터의 흐름을 시간순서에 의해 전달되어지는 스트림으로 처리한다. 이때, 스트림은 시간순서에 의해 전달되어진 값들의 collection으로 이해하면 된다. 
* 각각의 스트림은 새로 만들어져서(branch) 새로운 스트림이 될 수 있고, 여러개의 스트림의 합쳐질 수도 있다.(merge)
* 스트림은 map, filter와 같은 함수형 메소드를 이용하여 `immutable`하게 처리할 수 있다. 
* 스트림에 listening 함으로써 데이터의 결과값을 얻을 수 있는데, 이를 `subscribe`라고 표현한다. 

#### Observable & Observer
> An observer subscribes to an Observable. An Observable emits items or sends notifications to its observers by calling the observers’ methods.
> **[Reactive 홈페이지 - Observable 정의](http://reactivex.io/documentation/observable.html)**

* Observable은 observer의 메소드를 호출하면서 item이나 정보등을 호출하는 역할을 한다. Observer는 `onNext`, `onError`, `onCompleted`의 메소드가 구현되어 있다.
* Observer는 observable을 `subscribe`, 즉 listening 한다. Observer는 Subscriber, watcher, reactor로 불려진다.

#### Reactive Programming 이 필요한 이유
> Apps nowadays have an abundancy of real-time events of every kind that enable a highly interactive experience to the user. We need tools for properly dealing with that, and Reactive Programming is an answer.

요즘의 앱은 사용자의 모든 종류의 실시간 이벤트가 들어온다. 그것을 적절하게 다루기 위해서 Reactive Programming 이 필요하다. 
또한, Reactive Programming 은 Promise의 장점을 극대화 할 수 있다. Promise는 Observable과 개념적으로 유사한데, 차이가 있다면 Promise는 단 하나의 value를 다룰 수 있지만, Observable은 다수의 value를 다룰 수 있다. 

```
myObservable.subscribe(successFn, errorFn);
myPromise.then(successFn, errorFn);
```
> The Promise is an Observable The Observable is not a Promise

* Observable 은 A Stream에 의해 B Stream이 영향을 받는 경우, A만 바꾸어도 B가 자동으로 바꿀 수 있도록 구성할 수 있어서 **데이터의 동기화를 간편하게 할 수 있다.** 이렇게 할 수 있는 이유는 A와 B Stream의 관계를 `선언적`으로 선언했기 때문이다.  


#### Observable vs Iterable
Observable 과 Iterable의 본질은 같지만 데이터를 다루는 방식이 다르다. Observable 은 pull방식으로, Iterable은 push방식으로 데이터를 다룬다.

#### Functional Reactive Programming(FRP)
Reactive Programming은 Stream과 Observer 통해 이벤트들의 흐름들을 관찰할 수 있다. 그리고 이러한 흐름들을 이루는 데이터들로 무언가 할 수 있다는 것이 바로 Functional Reactive Programming인 것이다. 모든 데이터 흐름은 어디에서나 존재할 수 있고, 변수, 유저인풋, 속성, 캐시, 데이터 구조 등 모든 것이 데이터 흐름이 될 수 있다. 이러한 데이터의 흐름들을 새롭게 생성하고, 조합하고, 거를 수 있는 것이다. 

### 2. LINQ(Language-Intergrated Query) to Events
`LINQ`는 에릭 마이어가 만든 통합 질의 언어로 C# 3.0부터 등장한 개념이다. Rx에서는 이벤트와 LINQ의 개념을 결합한 Operator를 제공한다. 

```
// Query syntax
IEnumerable<int> numQuery1 = 
	from num in numbers
	where num % 2 == 0
	orderby num
	select num;

// Method syntax
IEnumerable<int> numQuery2 = numbers
	.Where(num => num % 2 == 0)
	.OrderBy(n => n);
```

#### Observable + LINQ
Mouse Move에 대한 이벤트로 들어오는 데이터들을 처리할 수 있는 아래의 예제를 통해 LINQ가 이벤트에 적용되는 것을 이해할수 있을 것이다. 

```javascript
// 1.
const targetElement = document.getElementById('dragElement');
const drag$ = Rx.Observable
	.fromEvent(targetElement, 'mouseup');

// 2.
drag$
	.map(ev => ({
		startX: ev.pageX,
		startY: ev.pageY
	}));

// 3.
drag$.subscribe(pos => {
	console.log(pos)
});
```

### 3. Scheduler
`Scheduler`는 비동기(멀티스레드) 환경에서 오퍼레이터 실행 시점을 제어해준다. 







