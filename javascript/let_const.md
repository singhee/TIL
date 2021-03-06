# ES6 In Depth : let 과 const 

## JS의 변수와 관련된 문제점
문제점 1. 블럭이 스코프를 결정하지 않는다
: JS 함수 안에서 선언한 var 의 스코프는 해당 함수 전체이다.
: var 의 스코프는 변수가 선언된 곳으로부터 양쪽(위 아래)으로 퍼져 나간다.
: var 로 선언된 변수는 함수 시작 시점에서 생성된다. (hoisting)

문제점 2. 루프 안의 변수가 과도하게 공유된다
: var 문제점을 해결하기 위해 나온것이 'let' => 모든 경우에 var 대신 let 을 사용하라.
	

## let이 var 과 다른 점
1. let 은 블럭을 기준으로 스코프를 결정한다(block-scoped)
- let 을 사용하더라도 hoisting 은 적용된다. ( 블럭 내에서의 hoisting )

2. 글로벌 let 변수는 글로벌 객체의 속성이 아니다. 
- window.variableName 코드로 접근 불가
- 웹 페이지 안에서 실행되는 모든 js 코드들을 둘러싸는 보이지 않는 개념적인 블럭 안에 존재한다.

3. for 형태의 루프는 루프를 반복할 때마다 매번 x 변수를 새로 바인딩 한다

4. let 변수를 선언 전에 참조하는 것은 에러다 (uninitialized)

5. let이 var 보다 약간 느릴 수 있다

6. let 변수 선언을 반복하면 SyntaxError 이다

## 또 하나의 키워드 도입 'const'
1. const 로 선언한 변수에는 값을 할당 할 수 없다. 오직, 변수를 선언하는 시점에만 값을 할당할 수 있다. (선언 시 값 할당 하지 않으면, Error)

2. 나머지는 성질은 let 과 동일하다.

### 요즘 `const` 사용법 [[원문]]](https://mathiasbynens.be/notes/es6-const)
Javascript 에서 변수 사용시 다음과 같이 하는것이 좋다.
1. 기본적으로는 `const`를 사용한다.
2. 재할당이 필요한 경우에만 `let`을 사용한다.
3. `var`는 ES2015 에서는 쓰지 않는다.

위의 말이 이상하거나 어색하게 느껴질 수도 있다. `const`는 상수에만 쓰는거 아닌가 하고 말이다. 맞다. `const`는 상수에만 쓰는 것이다. 그러므로 위의 1번의 말은 "기본적으로 상수를 사용하라"는 말로 표현할 수 있다. 왜 이렇게 해야할까? 원문에서는 `const`가 항상 같은 객체를 가리키므로 코드가 읽기 편해진다고 말하고 있다. 물론 맞는 말이다. 하지만, 근본적으로 접근해 보면 `const`를 우선시해서 쓴 코드를 읽게 된다면 `let`은 자연히 재할당하게 된다. 

## namespace
ES6 는 for 루프를 위한 스코프, 새로운 글로벌 let 스코프, 모듈 스코프, 그리 고 인자의 디폴트 값을 계산하기 위한 스코프를 추가했습니다.

## let 과 const 를 쓰려면
ES6 컴파일러를 사용해야 한다.

