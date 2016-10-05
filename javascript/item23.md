#아이템22. 가변 인자 함수를 생성하기 위해 arguments를 사용하라
	1. 가변 인자는 어떠한 명시적인 공식 파라미터도 정의하지 않는다.
	2. 가변 인자 함수를 구현하기 위해 자바스크립트의 모든 함수에 암묵적으로 전달되는 arguments 객체를 사용해라.
		* arguments 객체 : 실제 인자와 비슷한 인터페이스 제공  

```javascript
function average(){ // 가변인자 average 함수
	for(var i = 0, sum = 0, n= arguments.length; i < n; i++){
	sum += arguments[i];
	}
	return sum / n
}
```	
	3. 고정인자 버전의 가변인자 함수

```javascript	
function average(){
	return averageOfArray(arguments); 
}
```