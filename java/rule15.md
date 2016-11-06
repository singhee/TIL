# 규칙15. 변경 가능성을 최소화하라

`변경 불가능(Immutable)`클래스는 그 객체를 수정할 수 없는 클래스이다. 객체가 생성될 때 객체 내부의 정보가 주어지며, 객체가 살아있는 동안 그 정보는 그대로 유지된다. 자바 플랫폼 라이브러리에는 이런 클래스(Immutable Class)가 많다. `String` `기본 자료형 클래스`, `BigInteger`, `BigDecimal`등이 그런 클래스이다. 


### Immutable Class를 만드는 이유
Immutable Class는 Mutable Class보다 

	1. 설계하기 쉽다.
	2. 구현하기 쉽다.
	3. 사용하기도 쉽다. 
	4. 오류 가능성이 적고, 더 안전하다. 

### Immtable Class를 만들 때 따라야할 규칙
##### 1. 객체 상태를 변경하는 메소드(수정자 mutator 메소드 등)를 제공하지 않는다.
##### 2. 계승할 수 없도록 한다.
`Immutable Class`를 계승할 수 없도록 설계하면, 잘못 작성되거나 악의적인 하위 클래스가 객체 상태가 변경된 것처럼 동작해서 `변경 불가능성`을 깨뜨리는 일을 막을 수 있다. 계승을 금지하려면 보통 클래스를 `final`로 선언하면 된다.
##### 3. 모든 필드를 final로 선언한다.
모든 필드를 `final`로 선언하면, 시스템이 강제하는 형태대로 프로그래머의 의도가 분명히 드러난다. 또한, 새로 생성된 객체에 대한 참조가 `동기화(synchronization)`없이 다른 스레드로 전달되어도 안전하다.
##### 4. 모든 필드를 private로 선언한다.
모든 필드를 `private`로 선언하면 클라이언트가 필드가 참조하는 `Mutable Object`를 직접 수정하는 일을 막을 수 있다. `기본 자료형 값`, `Immutable Object`에 대한 참조 필드를 `public`으로 선언하는 `Immutable Class`를 만드는 것이 가능하지만, 이는 나중에 클래스의 내부 표현을 변경할 수 없게 되기 때문에 권장하지 않는다. - (규칙 13)
##### 5. 변경 가능 컴포넌트에 대한 독점적 접근권을 보장한다.
클래스에 포함된 `Mutable Object`에 대한 참조를 클라이언트는 획득할 수 없어야 한다. 그런 필드는 클라이언트가 제공하는 객체로 초기화해서는 안되고, `접근자(accessor)` 또한 그런 필드를 반환해서는 안된다. 따라서 생성자나 접근자, readObject 메소드 안에서는 `방어적 복사본(defensive copy)`를 만들어야 한다. - (규칙 39)

아래의 클래스는 접근자는 가지고 있지만 수정자는 가지고 있지 않다.
```java
public final class Complex{
	private final double re;
	private final double im;

	public Complex(double re, double im){
		this.re = re;
		this.im = im;
	}

	// 대응되는 수정자가 없는 접근자들
	public double realPart(){ return re; }
	public double imaginaryPart() { return im; }

	public Complex add(Complex c){
		return new Complex(re + c.re, im + c.im);
	}

	public Complex subtract(Complex c){
		return new Complex(re - c.re, im - c.im);
	}

	public Complex multiply(Complex c){
		return new Complex(re * c.re - im * c.im, re * c.im + im * c.re);
	}

	public Complex divide(Complex c){
		double tmp = c.re * c.re + c.im * c.im;
		return new Complex((re * c.re + im * c.im) / tmp, (im * c.re - re * c.im) / tmp);
	}
}
```



































































