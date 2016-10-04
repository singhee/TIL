#아이템4. 객체 래퍼보다 원시 데이터형을 우선시하라
1. 자바스크립트는 객체와 함께, 다섯 가지의 원시 데이터형 값을 가진다 
	- 불리언, 숫자, 문자열, null, undefined
		> typeof(null) : object type
		> ECMAScript의 null : null type

2. 불리언, 숫자, 문자열은 객체처럼 래핑되는 생성자가 제공된다
	
	```javascript
	var s = new String("hello")	// typeof "hello" -> string
								// typeof s -> object
	```

	- 같은 원시 데이터형 문자열을 가지고 있는 String객체는 내장 연산자를 사용해 비교할 수 없다
	  String객체는 개별 객체이기 때문에 자기자신과만 동일
		
	
	```javascript 
	var s1 = new String("hello");
	var s2 = new String("hello");
	s1 === s2; // false
	```

3. 원시 데이터형에 프로퍼티를 설정하거나 가져오면 암묵적으로 객체 래퍼(암묵적인 감싸기)를 생성
