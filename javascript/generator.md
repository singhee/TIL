# 제너레이터를 사용한 비동기 프로그래밍
자바스크립트가 다른 언어들과 구분되는 큰 특징 중의 하나는 **싱글스레드를 기반으로 하는 비동기 방식의 언어**라는 점이다. 그래서 자바스크립트는 Non-blocking IO를 사용하는 Node.js의 언어로 사용되면서 서버사이드에서도 큰 인기를 얻고 있다. 하지만, 이런 특징에서 오는 단점도 적지만은 않은데, 대표적인 것이 바로 연속적 전달 방식(CPS, Continuation Passing Style)으로 인한 콜백 지옥(Callback Hell)이다. 

이 콜백 지옥을 해결하기 위해, ES6에 `Promise`가 포함되면서 콜백 지옥의 문제들을 완화할 수 있게 되었다. 하지만 [Promise](https://github.com/singhee/TIL/blob/master/promise/promise.md)는 기대하는 것과는 다르게 콜백 지옥을 해결하기 위해서 나온 도구가 아니라, 단지 완화시킬 수 있는 방법을 제공해 줄 뿐이었다. 그리고 ES6에는 Promise말고도 비동기 프로그래밍을 위해 아주 중요한 개념인 `Generator`가 있다. 그렇다면 Generator에 대해서 알아보도록 하자.

## What is Generator?

### Generator
제너레이터는 함수의 실행을 중간에 멈추었다가(일시정지) 필요한 시점에 다시 재개(재시작)할 수 있는 일종의 `코루틴(Coroutine)`이라고 볼 수 있는데, 코루틴과는 조금 다른 점은 멈출 때 돌아갈 위치를 직접 지정할 수 없고, 단순히 호출자에게 제어권을 넘겨주는 것이다.

Generator의 반복이 끝나는 시점은 다음과 같이 3가지 경우이다.

1. generator 함수에서 `return` 사용
2. 에러의 발생
3. 함수의 끝부분까지 모두 수행된 이후

그리고 반복이 다 끝난다면 Generator의 done프로퍼티가 true가 된다. 이러한 generator함수는 Generator객체를 반환하며, `function*` 키워드로 정의할 수 있다.

### Generator Methods
* `next(value)`
이 메서드는 다음 값을 얻는 역할을 하고, Iterator의 `next`메서드와 유사하지만, optional argument를 받는 점이 다르다. 이 메서드의 매개변수는 바로 이전의 yield [expression]의 반환값으로 사용된다.

* `return(value)`
이 메서드는 매개변수로 온 값을 value로써 반환하며, Generator를 종료시킨다.
`{value: value, donr: true}`

* `throw(exception)`
이 메서드는 인자로 받은 에러를 발생시키고, Generator를 종료시킨다. generator함수 내부의 catch 구문을 통해 처리할 수도 있다. 또한 내부에서 catch 구문으로 처리한 경우 Generator는 종료되지 않는다. 

```javascript
function* myGen() {
	yield 1;
	yield 2;
	yield 3;
	return 4; 
}

const myItr = myGen(); // 이터레이터 객체 반환
console.log(myItr.next()); // {value:1, done:false}
console.log(myItr.next()); // {value:2, done:false}
console.log(myItr.next()); // {value:3, done:false}
console.log(myItr.next()); // {value:4, done:true}
```

위에서 myGen 제너레이터는 실행될 때 이터레이터 객체를 반환한다. 그리고 이터레이터의  `next()` 함수가 호출될 때마다 호출되는 곳의 위치를 기억해둔 채로 실행된다. 그리고 함수 내부에서 `yield`를 만날 때마다 기억해둔 위치로 제어권을 넘겨준다 .  이런 식으로 `next()` -> `yield` -> `next()` -> `yield` 라는 순환 흐름이 만들어 지고, 이 흐름을 따라 해당 함수가 끝날 때까지 진행된다.

여기에서 중요한 점은  `next()`와  `yield`가 서로 데이터를 주고받을  수 있다는 점이다. 위의 예제에서 볼 수 있듯이 `yield`키워드 뒤의 값은 `next()`함수의 반환값으로 전달된다.  그럼 반대로 호출자가 제너레이터에게 값을 전달할 수 있을까? 물론 가능하다. `next()`를 호출할 때 인수를 넘기면 된다. 

```javascript
function *myGen() {
    const x = yield 1;       // x = 10
    const y = yield (x + 1); // y = 20
    const z = yield (y + 2); // z = 30
    return x + y + z;
}

const myItr = myGen();
console.log(myitr.next());   // {value:1, done:false}
console.log(myitr.next(10)); // {value:11, done:false}
console.log(myitr.next(20)); // {value:22, done:false}
console.log(myitr.next(30)); // {value:60, done:true}
```

위의 myGen 함수는 내부에서 콜백도 없고 Promise도 없지만, 비동기적으로 데이터를 주고받으며 실행되고 있다는 것을 볼 수 있다. 

## 콜백 지옥
콜백 지옥을 소환해 보자.
```javascript
function getId(phoneNumber, callback) { /* … */ }
function getEmail(id, callback) { /* … */ }
function getName(email, callback) { /* … */ }
function order(name, menu, callback) { /* … */ }

function orderCoffee(phoneNumber, callback) {
    getId(phoneNumber, function(id) {
        getEmail(id, function(email) {
            getName(email, function(name) {
                order(name, 'coffee', function(result) {
                    callback(result);
                });
            });
        });
    });
}
```
위의 코드는 단순히 콜백의 문제인 들여쓰기와 가독성의 문제만 있는것은 아니다.  더 중요한 문제점은 콜백함수를 다른 함수로 전달하는 순간 그 콜백함수에 대한 제어권을 잃는다는 점이다.  그리고 처음에 제공하고자 하는 콜백 함수는 한없이 위임되어 한없이 콜백 지옥의 끝에 파묻혀 있다. 이로 인해 프로그램이 더 예측하기 어렵게 되고 에러가 발생하기 쉬우며, 디버깅 또한 어려워진다. 

## 프로미스의 구원
하지만 앞서 얘기했듯이 ES6 의 Promise의 등장으로 이러한 문제는 상당 부분 완화되었다.  위의 콜백 지옥을 Promise로 완화시켜 보도록 하자.  먼저, getXXX 함수에서의 콜백 파라미터를 제거하고, 실행 결과로 Promise를 반환한다고 가정하자.
```javascript
function getId(phoneNumber) { /* … */ }
function getEmail(id) { /* … */ }
function getName(email) { /* … */ }
function order(name, menu) { /* … */ }

function orderCoffee(phoneNumber) {
    return getId(phoneNumber).then(function(id) {
        return getEmail(id);
    }).then(function(email) {
        return getName(email);
    }).then(function(name) {
        return order(name, 'coffee');
    });
}
```
가독성이 훨씬 나아진 것을 볼 수 있을 것이다. 뿐만 아니라, 해당 함수가 처리를 성공적으로 완료했을 경우 항상 `then()`에 넘겨진 함수가 단 한번만 실행될 것이라는 신뢰가 생겼다. 여기서 ES6의 Arrow 함수를 써서 좀 더 세련되게 코드를 작성할 수 있다. 아래의 코드를 보자

```javascript
function orderCoffee(phoneNumber) {
    return getId(phoneNumber)
        .then(id => getEmail(id))
        .then(email => getName(email))
        .then(name => order(name, 'coffee'));
}
```

## 비동기 코드를 동기식 코드처럼 작성하기 
아래의 코드와 위에서 Promise로 만든 코드를 비교해 보도록 해보자. 둘은 같은 동작을 하는 코드이다. 하지만, 둘은 비교했을 때 어떤 코드가 이해하기 더 쉬울까? 당연히 아래의 코드가 훨씬 직관적이고 알아보기 쉽다. Promise 코드에서 아래와 같이 할 수 없는 이유는 처음에도 언급했듯이 자바스크립트가 싱글-스레드 기반의 언어이기 때문이다. 또한, 각각의 네트워크 요청이 값을 반환하기 전까지 프로그램 전체가 멈춰서 대기를 해야한다면 이 프로그램은 너무 느려서 사용할 수가 없을 것이다. 

```javascript
function getId(phoneNumber) { /* … */ }
function getEmail(id) { /* … */ }
function getName(email) { /* … */ }
function order(name, menu) { /* … */ }

function orderCoffee(phoneNumber) {
    const id = getId(phoneNumber);
    const email = getEmail(id);
    const name = getName(email);
    const result = order(name, 'coffee');
    return result;
}
```
하지만, 여기서 Generator를 사용해본다면 어떨까? 그렇다. 우리는 Generator의 활용을 위해 여기까지 온 것이다. 제너레이터는 함수를 실행 도중에 멈추고 제어권을 다른 곳으로 넘겨줄 수 있으며 값도 전달할 수 있다. 그렇다면 전체 프로그램을 멈추지 않고도 위의 방식의 코드를 작성할 수 있지 않을까?

그렇다면, 우선 간단하게 기존의 함수 선언에 `*` 을 추가해서 제너레이터로 변경하고, 각 할당문에 `yield`를 추가해 보도록 하자. 

```javascript
function* orderCoffee(phoneNumber) {
    const id = yield getId(phoneNumber);
    const email = yield getEmail(id);
    const name = yield getName(email);
    const result = yield order(name, 'coffee');
    return result;
}
```



