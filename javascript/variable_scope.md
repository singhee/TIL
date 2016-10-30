# javascript 의 변수 선언과 유효범위

### 변수 선언
javascript 에서 변수는 `var` 를 이용해 여러 개의 변수를 선언할 수 있으며, 변수를 선언함과 동시에 초기화할 수도 있다.
```javascript
var x;
var y = 10;
var a, b, c;
var i = 10, j = 10, k = 10;
```

하지만 변수에 초기값을 지정하지 않으면 초기값은 `undefined` 가 된다.

### 자바스크립트에는 명시적인 타입이 없다
javascript 는 다른 java 나 c 같은 언어와 달리 명시적인 타입이 없다. javascript 변수는 어떤 자료형의 값도 담을 수 있으며, 한 변수를 다른 타입의 값으로 할당할 수 있다.

```javascript
var id = 10; 	 // number
id = "ten";  	 // string
console.log(id); // ten

```

### 변수의 Global범위와 Local범위
javascript 변수의 유효범위는 변수를 어디에서 접근할 수 있느냐를 나타낸다.
	- Global 범위는 코드 내에서 어디서든 변수에 접근할 수 있음을 의미한다.
	- Local 범위는 함수 내에서 변수를 정의하고 접근할 수 있음을 의미한다.
	- 함수 매개변수도 Local 변수로 간주하며 해당 함수의 본문 내에서 접근할 수 있다.
	- local 변수와 global 변수의 이름이 같을 경우 local 변수가 우선순위가 높다.

```javascript
var name = "global";    // 전역 변수를 선언
function checkscope(){  
    var name = "local"; // 지역 변수를 선언
    console.log(name);  // 전역 변수가 아닌 지역 변수를 사용
}
checkscope();           // 출력 결과: "local"   
```

##### `var` 키워드 없이 변수를 선언하면 자동으로 Global 변수가 된다.

```javascript
var name = "global";    // 전역 변수를 선언
function checkscope(){  
    name = "local";     // 전역 변수를 변경
    name2 = "local";    // 암묵적으로 새 전역 변수가 선언됨
}
checkscope();
console.log(name);      // output: "local" 
console.log(name2);     // output: "local" 
```

##### 중첩 함수에서 내부 함수는 그것이 담긴 함수의 변수에 접근할 수 있다.
```javascript
function changeName(name){          // "name"은 지역 변수다
    function inner1(){
        name = name + "-inner1";     
        function inner2(){
            name = name + "-inner2";
        }
        inner2();
    }
    inner1();
    return name;
}
console.log(changeName("Hello"));   // Hello-inner1-inner2
```

##### 자바스크립트에는 블럭 유효범위가 없다
다른 java 나 c 같은 언어와 달리 블럭 `({})` 안에서 선언된 변수는 해당 블럭이 닫힌 이후에도 접근할 수 있다.
```javascript
{
    var count = 10;
}
console.log(count);     // 10
```

반복문 내에서 선언된 변수도 해당 반복문이 끝난 이후에도 접근할 수 있다.
```javascript
for (var i=0; i<11; i++){}
console.log( i );       // 11
```






