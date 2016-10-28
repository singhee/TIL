# Closure (클로저)

#### 클로저란?

클로저는 포함하고 있는 외부함수의 변수에 접근할 수 있는 내부 함수를 말한다. 스코프 체인(scope chain)으로 표현되기도 한다. 클로저는 세 가지 스코프 체인을 가지는데 먼저 클로저 자신에 대한 접근(자신의 블럭내에 정의된 변수), 외부 함수의 변수에 대한 접근, 전역변수에 대한 접근 이렇게 3단계로 구분할 수 있다.

내부 함수는 외부 함수의 변수뿐만 아니라 파라미터에도 접근할 수 있다. 단, 내부 함수는 외부 함수의 arguments 객체를 호출할 수는 없다. 

``` javascript
function showName(firstName, lastName) {
    var nameIntro = "Your name is ";
    // 이 내부 함수는 외부함수의 변수뿐만 아니라 파라미터 까지 사용할 수 있다.
    function makeFullName() {
        return nameIntro + firstName + " " + lastName;
    }
    return makeFullName();
}
showName("Michael", "Jackson"); // Your name is Michael Jackson
```

클로저는 Node.js 의 비동기, Non-blocking 아키텍처의 핵심기능으로 활용되고 있다. 

#### 클로저 규칙과 부수 효과
	
	규칙 1. 클로저는 외부함수가 리턴된 이후에도 외부함수의 변수에 접근할 수 있다.

``` javascript
function celebrityName(firstName) {
    var nameIntro = "This is celebrity is ";
    function lastName(theLastName) {
        return nameIntro + firstName + " " + theLastName;
    }
    return lastName;
}
var mjName = celebrityName("Michael"); // 여기서 celebrityName 외부함수가 리턴된다.
// 외부함수가 위에서 리턴된 후에, 클로저(lastName)가 호출된다.
// 아직, 클로저는 외부함수의 변수와 파라미터에 접근 가능하다.
mjName("Jackson"); // This celebrity is Michael Jackson
```

	규칙 2. 클로저는 외부 함수의 벼수에 대한 참조를 저장한다.

클로저는 실제 값을 저장하지 않는다. 클로저가 호출되기 전에 외부함수의 변수가 변경된다면, 이는 또다른 새로운 방법으로 활용될 수 있다. - 내부(Private) 변수

```javascript
function celebrityID() {
    var celebrityID = 999;
    // 우리는 몇개의 내부 함수를 가진 객체를 리턴할것이다.
    // 모든 내부함수는 외부변수에 접근할 수 있다.
    return {
        getID: function() {
            // 이 내부함수는 갱신된 celebrityID변수를 리턴한다.
            // 이것은 changeThdID함수가 값을 변경한 이후에도 celebrityID의 현재값을 리턴한다.
            return celebrityID;
        },
        setID: function(theNewID) {
            // 이 내부함수는 외부함수의 값을 언제든지 변경할 것이다.
            celebrityID = theNewID;
        }
    }
}
var mjID = celebrityID(); // 이 시점에, celebrityID외부 함수가 리턴된다.
mjID.getID(); // 999
mjID.setID(567); // 외부함수의 변수를 변경한다.
mjID.getID(); // 567; 변경된 celebrityID변수를 리턴한다.

```

####클로저 와 for문의 사용
클로저가 갱신된 외부함수의 변수에 접근함으로써, 외부 함수의 변수가 for 문에 의해 변경될 경우 의도치 않는 버그가 발생할 수 있다.

``` javascript
function celebrityIDCreator(theCelebrities) {
    var i;
    var uniqueID = 100;
    for (i=0; i < theCelebrities.length; i++) {
        theCelebrities[i]["id"] = function() {
            return uniqueID + i;
        }
    }
    return theCelebrities;
}

var actionCelebs = [{name:"Stallone", id:0}, {name:"Cruise", id:0}, {name:"Willis", id:0}];
var createIdForActionCelebs = celebrityIDCreator(actionCelebs);
var stalloneID = createIdForActionCelebs[0];
console.log(stalloneID.id); // 103

```

위에서, 익명의 내부함수가 실행될 시점에 i 값은 3이다(배열의 크기만큼 증가한 값). 숫자 3은 uniqueID 에 더해져 모든 celebritiesID 에 103 을 할당한다. 그래서 기대(100,101,102)와 달리 모든 리턴된 배열의 id = 103 이 된다.

이러한 결과가 나온 이유는 클로저는 외부변수에 대해 값이 아닌 참조로 접근하기 때문이다. 즉, 클로저는 최종 갱신된 변수 i 에 대해서만 접근할 수 있으므로, 외부 함수가 전체 for 문을 실행하고 리턴한 최종 i의 값을 리턴하게 된다.

이러한 부작용을 고치기 위해서 'Immediately Invoked Function Expression(IIFE)' 를 사용할 수 있다.

```javascript
function celebrityIDCreator(theCelebrities) {
    var i;
    var uniqueID = 100;
    for (i=0; i < theCelebrities.length; i++) {
        theCelebrities[i]["id"] = function(j) {
            // j 파라미터는 호출시 즉시 넘겨받은(IIFE) i의 값이 된다.
            return function() {
                // for문이 순환할때마다 현재 i의 값을 넘겨주고, 배열에 저장한다.
                return uniqueID + j;
            } () // 함수의 마지막에 ()를 추가함으로써 함수를 리턴하는 대신 함수를 즉시 실행하고 그 결과값을 리턴한다.
        } (i); // i 변수를 파라미터로 즉시 함수를 호출한다.
    }
    return theCelebrities;
}
var actionCelebs = [{name:"Stallone", id:0}, {name:"Cruise", id:0}, {name:"Willis", id:0}];
var createIdForActionCelebs = celebrityIDCreator(actionCelebs);
var stalloneID = createIdForActionCelebs[0];
console.log(stalloneID.id); // 100
var cruiseID = createIdForActionCelebs[1];
console.log(cruiseID.id); // 101

```

[Understand Javascipt Closures With Ease]("http://javascriptissexy.com/understand-javascript-closures-with-ease/")















