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

이 클래스는 복소스를 표현하는 클래스이다. Object 클래스가 제공하는 메소드 외에도 실수부와 허수부 값을 가져오기 위한 접근자, 그리고 사칙연산 각각에 대응되는 메소드를 제공하고 있다. 이것은 대부분의 `Immutable Class`가 따르는 패턴이다. `함수형 접근법(functional approach)`로도 알려져 있는데, 피연산자를 변경하는 대신, 연산을 적용한 결과를 새롭게 만들어 반환한다. `절차적(Procedural)` 또는 `명령형(imperative)` 접근법은 피연산자에 일정한 절차를 적용하여 그 상태를 바꾼다. 함수형 접근법은 `Immutable`을 보장하므로 장점이 많다. 

### Immutable Object 의 장점
##### 1. 변경 불가능 객체는 단순하다.
변경 불가능 객체는 생성될 때 부여된 한 가지 상태만을 갖는다. 반면 변경 가능한 객체의 상태는 까다롭게 바뀔 수 있다. 수정자 호출 시 객체 상태가 어떻게 바뀌는지 정확하게 기술되어 있지 않은 변경 가능 클래스는 안정적으로 사용하기 어렵거나 아예 불가능하다.
##### 2. 변경 불가능 객체는 스레드에 안전(thread-safe)하다.
스레드에 안전하면 어떤 동기화도 필요없으며, 여러 스레드가 동시에 사용해도 상태가 훼손될 일이 없다.
##### 3. 변경 불가능한 객체는 자유롭게 공유할 수 있다.
스레드는 다른 스레드가 변경 불가능한 객체에 무슨직을 하는지 알 수 없다. 변경 불가능 클래스는 클라이언트가 기존 객체를 재사용하도록 적극 장려해서 이 장점을 살릴 필요가 있다. 그렇게 하는 한 가지 쉬운 방법은, 자주 사용되는 값을 `public static final` 상수로 만들어 제공하는 것이다. 이 방법은 한 단계 더 개선할 수 있다. 변경 불가능 클래슨는 자주 사용하는 객체를 `캐시(cache)`하여 이미 있는 객체가 거듭 생성되지 않도록 하는 [정적 팩터리(규칙 1)]()를 제공할 수 있다.
##### 4. 변경 불가능한 객체는 그 내부도 공유할 수 있다.
##### 5. 변경 불가능 객체는 다른 객체의 구성요소로도 훌륭하다.
구성요소들의 상태가 변경되지 않는 객체는 복잡하다 해도 훨씬 쉽게 `불변식(invarient)`을 준수할 수 있다. 그 사례로, `맵(map)`과 `집합(set)`을 들 수 있다. 변경 불가능 객체는 맵의 키나 집합의 원소로 활용하기 좋다. 한번 집어넣고 나면 그 값이 변경되어 맵이나 집합의 불변식이 깨질 걱정은 하지 않아도 된다.

### Immutable Object 의 단점
##### 1. 변경 불가능 객체의 유일한 단점은 값마다 별도의 객체를 만들어야 한다는 점이다.
이는 객체 생성 비용이 높을 가능성이 있다. 큰 객체라면 특히 더 그러하다.