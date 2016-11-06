# Factory Pattern (팩토리 패턴)
`factory pattern`은 super class와 여러개의 sub class가 있는 상황에서 input이 발생하면 하나의 sub class를 반환해야 할 때 사용된다. factory class는 client class로 부터 인스턴스를 생성하는 책임을 가져온다.

## Super Class (슈퍼 클래스)
factory pattern에서 super class는 interface, abstract class 또는 일반적인 java class가 될 수 있다. 

```java
public abstract class Product{
	public abstract String getName();
	public abstract int getPrice();

	@Override
	public String toString(){
		return "product name : " + getName() + ", price : " + getPrice();
	}
}
```

## Sub Class (서브 클래스)
Product class를 상속받은 Computer와 Ticket class를 구현한다. 아래의 클래스들은 super class 하위의 sub class 를 정의한 것이다.
```java
public class Computer extends Product{
	private String name;
	private int price;

	public Computer(String name, int price){
		this.name = name;
		this.price = price;
	}

	@Override
	public String getName(){
		return this.name;
	}

	@Override
	public int getPrice(){
		return this.price;
	}

	public void toString(){
		System.out.println("항목 :: " + this.name + ", 가격 :: " + this.price);
	}
}
```
```java
public class Ticket extends Product{
	private String name;
	private int price;

	public Ticket(String name, int price){
		this.name = name;
		this.price = price;
	}

	@Override 
	public String getName(){
		return this.name;
	}

	@Override
	public int getPrice(){
		return this.price;
	}

	public void toString(){
		System.out.println("항목 :: " + this.name + ", 가격 :: " + this.price);
	}
}
```

## Factory Class (팩토리 클래스)
super class와 sub class가 완성되었으니, factory class를 작성해보자.
```java
public class ProductFactory{
	public static Product getProduct(String type, String name, int price){
		if("ticket".equals(type))
			return new Ticket(name, price);
		else if("computer".equals(type))
			return new Computer(name, price);
		return null;
	}
}
```

>
factory class를 static으로 선언함으로써 singleton을 유지할 수가 있다.
input parameter에 의해 sub class가 생성되어 리턴된다.


아래는 factory pattern을 활용한 테스트 코드이다.

```java
public class main{
	public static void main(String[] args){
		Product t1 = ProductFactory.getProduct("ticket", "한국여행", 300000);
		Product t2 = ProductFactory.getProduct("computer", "pc", 1500000);

		System.out.println(t1.toString());
		System.out.pringln(t2.toString());
	}
}
```

>
Factory Pattern은 구현체 보다는 인터페이스 코드 접근에 좀 더 용의하게 해준다.
Factory Pattern은 클라이언트 클래스로부터 인스턴스를 구현하는 코드를 떼어내서, 코드를 더욱 탄탄하게 하고 결합도를 낮춘다.
Factory Pattern은 구현과 클라이언트 클래스들의 상속을 통해 추상적인 개념을 제공한다.

>
Factory Pattern은 객체를 생성하기 위해 인터페이스를 정의하지만, 어떤 클래스의 객체를 생성할 지에 대해서는 하위 클래스에서 결정한다. (클래스의 상속을 이용한다)

#### 활용 예
	1. 생성할 객체 타입을 예측할 수 없을 때 : 부모 클래스 타입을 이용한다
	2. 생성할 객체의 명세를 하위 클래스에서 정의하고자 하는 경우
	3. 객체 생성의 책임을 하위 클래스에 위임시키고 어느 하위 클래스가 위임했는지에 대한 정보를 은닉하고자 하는 경우


