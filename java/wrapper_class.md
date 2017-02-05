# Wrapper class
Wrapper Class는 사용할 때 Caching 관련 이슈에 대해서 주의해야 한다. 
```java
Integer a = 127;
Integer b = 127;
Integer c = 128;
Integer d = 128;

System.out.println(a==b);	// true
System.out.println(c==d);	// false
```

Integer Class 를 살펴보게되면 IntergerCache라는 static class로 자주 쓰는 범위에 대한 값들을 Caching을 하고 있다. 즉, Wrapper Class 는 자주 사용하는 범위에 대해 기본값으로 정해놓고 Caching 을 하게되는 것이다.

```java
Integer i2 = Integer.valueOf(2);
```
위와 같이 object를 생성한다면 이미 해당 범위의 숫자까지는 내부적으로 Integer[] cache 에 담아 놓은 상태라 cached object를 반환한다. 동일한 object를 반환받았으니 객체비교인 == 에서 true가 나온 이유인 것이다.



