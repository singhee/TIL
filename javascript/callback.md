# Javascript - Callback 
#####[출처](http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/ 

## Learn JavaScript Higher-order Functions-Callback Functions
자바스크립트에서 함수는 **일급객체**이다. 일급 객체가 되기 위해서는 다음과 같이 몇 가지 조건을 만족해야 한다. 
1. stored in variables 
2. passed as arguments to functions
3. created within functions, 런타임에 생성될 수 있다. 
4. returned from functions

자바스크립트에서는 함수가 일급객체이기 때문에 함수를 인자로 사용하는 콜백패턴을 사용할 수 있다. 콜백함수는 함수형 프로그래밍에서 발생한 패러다임이다. 함수형 프로그래밍은 **함수를 인자처럼 사용하는 것**인데 콜백함수가 함수형 프로그래밍에서의 가장 일반적인 기술에 해당한다. 

## What is a Callback or Higher-order Function?
콜백함수는 higher-order function(고차함수) 이라고 알려져 있으며, 파라미터로 함수를 전달한다. 그리고 전달받은 함수를 함수의 내부에서 실행시킨다. 

```javascript
$("#btn_1").click(function() { 
	alert("Btn 1 Clicked"); 
});
```
위에서처럼 click함수의 인자로 함수를 전달하고 있으며, 그 함수는 click함수가 실행이 되면 동작을 하게 된다. 이 형태가 가장 전형적인 자바스크립트의 콜백함수이다. 

## How Callback Functions Work?
이제부터는 콜백함수가 어떻게 동작하는지에 대해 알아보도록 하자. 자바스크립트에서 모든 것은 객체이다. 함수마저 객체이다.(일급 객체) 그러기 때문에 함수를 변수처럼 사용할 수 있어, 함수를 콜백으로 다른 함수의 인자처럼 사용 할 경우에는 오직 함수의 정의만을 넘겨주면 된다. 즉, 아래와 같이 함수의 이름만을 넘겨주면 된다. 
```javascript
setInterval(callback, 1000)
```
위의 코드처럼 함수의 인자로 전달된 함수는 언제든 원하는 시점에 실행을 시킬수가 있다. **즉, 콜백함수는 전달 받은 즉시 바로 실행이 될 필요가 없다는 말이며, 함수의 내부의 어느 특정시점에 실행이 가능하다는 것이다.**  

## Callback Functions Are Closures
우리가 어떤 함수의 인자로 콜백함수를 전달할 때, 전달받은 함수의 특정시점에 그 콜백함수가 동작을 할 것이다. **마치 전달받은 함수가 이미 콜백함수를 내부에서 정의 한 것처럼 말이다.** 이 말은 **콜백은 클로저**라는 말과 같다. 전달된 콜백함수는 콜백함수를 포함한 함수 내부의 인자에 접근이 가능하고, 심지어 전역변수에도 접근이 가능한 상태가 된다. 즉, 콜백함수로 인해 기존의 함수가 가진 Scope에 새로운 내부 Scope가 추가되는 것이다.

## Basic Principles when Implementing Callback Functions
콜백함수를 적용하기 위해서는 몇 가지 원칙이 있다.
### 1. Use Named OR Anonymous Functions as Callbacks : 이름이나 익명의 함수를 사용하라.
### 2. Pass Parameters to Callback Functions : 콜백함수로 파라미터를 전달하라.
### 3. Make Sure Callback is a Function Before Executing It : 콜백함수가 실행 되기 전에 함수임을 명확하게 하라.
### 4. Problem When Using Methods With The this Object as Callbacks : this를 사용한 메서드를 콜백으로 사용시 문제가 된다. 
### 5. Use the Call or Apply Function To Preserve this : Call 과 Apply를 통한 this 보호

## "Callback Hell" Problem And Solution
간단한 실행을 위한 비동기적인 코드는 수많은 콜백함수의 연속으로 이어지는 경우가 있다. 아래의 코드가 바로 콜백지옥이다. 이러한 코드는 가독성이 매우 떨어지게 된다. 
```javascript
var p_client = new Db('integration_tests_20', new Server("127.0.0.1", 27017, {}), {'pk':CustomPKFactory}); 
p_client.open(function(err, p_client) { 
	p_client.dropDatabase(function(err, done) { 
		p_client.createCollection('test_custom_key', function(err, collection) { 
			collection.insert({'a':1}, function(err, docs) {
				 collection.find({'_id':new ObjectID("aaaaaaaaaaaa")}, function(err, cursor) { 
				 	cursor.toArray(function(err, items) {
				 		test.assertEquals(1, items.length); // Let's close the db 
				 		
				 		p_client.close(); 
				 	}); 
				}); 
			}); 
		}); 
	}); 
});
```
위의 코드는 node.js로 서버사이드를 처리하다 보면 의도치 않게 이러한 처리들을 하게 되는 경우가 빈번하게 생긴다.