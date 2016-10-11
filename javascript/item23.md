# 아이템23. 절대 arguments 객체를 수정하지 마라

1. arguments는 항상 배열처럼 동작하지 않는다.
2. 함수의 arguments 객체와, 이름이 지정된 파라미터 사이의 관계는 불안정하다.
3. strict mode 에서 함수 파라미터는 자신의 arguments 객체를 별명으로 삼지 않는다.
 
######  arguments 객체는 절대로 수정하지 않는 편이 안전하다!
######  arguments 객체를 수정하고 싶다면, arguments 객체의 요소들을 진짜 배열로 복사하면 된다. [ ].slice.call(arguments)

```javascript
// arguments 복사 구현
var args = [].slice.call(arguments); //  표준 Array 타입 인스턴스 반환
```