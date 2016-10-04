# Promise 란 무엇인가
	전통적인 콜백 패턴은 비동기 처리 중 발생한 오류를 예외 처리하기 힘들고 여러 개의 비동기 로직을 한꺼번에 처리하는 데 한계가 있다. 
	Promise 는 비동기 처리 로직을 추상화한 객체로 전통적인 콜백 패턴이 가진 단점을 일부 보완하며 비동기 처리 시점을 명확하게 표현한다.

	=> 복잡한 비동기 처리를 쉽게 패턴화 한다.**

##### - 일반적인 자바스크립트에서의 비동기 처리(콜백사용) 
```javascript
getAsync("fileA.txt", function(error, result){
	if(err){ // 실패 시
		throw error;
	}
	// 성공 시
})

```

--------
## Promise API
###### - Constructor : 생성자 함수에 `new` 연산자를 선언하여 promise 객체를 생성해 사용한다.
	var promise = new Promise(function(resolve, reject){
		// 비동기 처리 작성
		// 처리 끝나면 resolve 또는 reject 호출
	})


###### - Instance Method  promise 인스턴스 객체는 성공`resolve` 또는 실패`reject` 했을 때 호출될 콜백 함수를 `promise.then()` 을 통해 등록할 수 있다.
	promise.then(onFulfilled, onRejected)
	// 성공 시 onFulfilled 호출
	// 실패 시 onRejected 호출
	// 선택자 인자(optional parameter)로 생략 가능

	// 오류 처리만 하고 싶은 경우
	promise.then(undefined, onRejected)
	promise.catch(onRejected) // 권장 - (예외 처리가 되지않는 onRejected)



###### - Static Method : 보조 메서드
	Promise.all()
	Promise.resolve()



## Promise 워크플로우

###### --- asyncFunction() 
	** Promise 생성자를 `new` 연산하여 promise 객체 반환 
	** 비동기 처리 성공시 호출될 콜백을 `then()` 으로 등록 / 실패시 호출될 콜백은 `catch()` 로 등록 

```javascript
function asyncFunction(){
	return new Promise(function(resolve, reject){
		resolve('hello');
	});
}

asyncFunction().then(function(value){
	console.log(value); // 'hello'
}).catch(function(error){
	console.log(error);
});

```


## Promise 상태

###### --- `new` 연산으로 생성한 promise 객체의 상태(state)

ES6 Promise | Promise/A+ |  |
----------------|----------|------------|
unresolved | **Pending** | 성공도 실패도 아닌 상태, promise 객체가 생성된 초기 상태
has-resolution | **Fulfilled** | 성공(resolve) 했을 때의 상태, onFulfilled 호출
has-rejection | **Rejected** | 실패(reject) 했을 때의 상태, onRejected 호출


###### --- Settled(불변) 상태	: 성공 또는 실패 했을 때의 상태
	promise 객체는 Pending 상태에서 Fulfilled 나 Rejected 상태가 되면 다시는 변화하지 않는다. [Settled(불변) 상태]
	=> then() 으로 등록한 콜백 함수는 한 번만 호출된다.


----

## Promise 객체 생성
	1. new Promise(function) 으로 promise 객체 생성
	2. function 에는 비동기 처리 작성
	  - 처리 결과 정상 resolve(결과 값) 호출
	  - 처리 결과 비정상 reject(error 객체) 호출


## Promise 객체 사용
	resolve()나 reject()가 호출될 때 실행될 콜백을 promise 객체에 등록한다.
	- onFulfilled : promise 객체에서 resolve() 호출되었을 때 처리
	- onRejected : promise 객체에서 reject() 호출되었을 때 처리




























