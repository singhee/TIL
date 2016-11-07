#String

Immutable Object (불변객체) <------> Mutable Object (가변객체)
	
1. 객체가 생성된 이후로 내부 상태가 변경되지 않는다.
2. Setter 가 존재하지 않는다.

```
장점 1. 스레드에 안전하다
장점 2. 객체의 상태변경, 보안에 안전하다
[Effectice Java 규칙.15](https://github.com/singhee/TIL/blob/master/java/rule15.md)
```

```
단점 1. 새로운 상태에 따른 새로운 객체가 생성되어야 한다
```
