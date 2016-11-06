# Abstract Factory Pattern (추상 팩토리 패턴)
`abstract factory pattern`은 생산적인 디자인 패턴 중 하나로써 `factory`를 좀 더 생산적으로 만들어 낼 수 있다는 점 외에는 `factory pattern`과 매우 비슷하다.

보통 `factory design pattern`에서는 어떠한 input에 대해 factory class안에서 `if-else`로 다른 sub class를 반환하는 일련의 과정을 통해 목적을 달성할 것이다.

abstract factory pattern에서는 `if-else`구문을 없애고, sub class마다 factory class를 가지게 하고 abstract factory에서는 input factory class를 통해 해당 sub class를 반환한다. 

## Super Class 와 Sub Class
```java
public abstract class Product {
    public abstract String getName();
    public abstract int getPrice();

    @Override
    public String toString() {
        return "product name : " + getName() + ", price :" + getPrice();
    }
}
```

```java
public class Computer extends Product {
    private String name;
    private int price;

    public Computer (String name, int price) {
        this.name = name;
        this.price = price;
    }

    @Override
    public String getName() {
        return this.name;
    }
    @Override
    public int getPrice() {
        return this.price;
    }
}
```

```java
public class Ticket extends Product {
    private String name;
    private int price;

    public Ticket (String name, int price) {
        this.name = name;
        this.price = price;
    }

    @Override
    public String getName() {
        return this.name;
    }
    @Override
    public int getPrice () {
        return this.price;
    }
}
```

## Factory Class마다 각각의 Sub Class
먼저 abstract factory interface(또는 abstract)가 필요하다
```java
public interface ProductAbstractFactory{
	public Product createProduct();
}
```
`createProduct()`는 super class 격인 Product 인스턴스를 반환한다. 
ProductAbstractFactory 인터페이스와 각각의 sub class를 구현해보자.

sub class를 위한 factory class는 다음과 같다
```java
public class ComputerFactory implements ProductAbstractFactory{
	private String name;
	private int price;	

	public ComputerFactory(String name, int price){
		this.name = name;
		this.price = price;
	}

	public Product createProduct(){
		return new Computer(name, price);
	}
}
```

각각의 sub class 는 비슷한 구조를 가진다.
```java
public class TicketFactory implements ProductAbstractFactory{
	private String name;
	private int price;

	public TicketFactory(String naem, int price){
		this.name = name;
		this.price = price;
	}

	public Product createProduct(){
		return new Ticket(naem, price);
	}
}
```

## 사용 클래스
이제 클라이언트 클래스에서 sub class생성 시점을 제공해주는 사용 클래스를 만들어보자
```java
public class ProductFactory{
	public static Product getProduct(ProductAbstractFactory product){
		return product.createProduct();
	}
}
```
getProduct method는 인자로 ComputerAbstractFactory를 받고 Product object를 반환한다. 이 코드가 object구현을 매우 깔끔하게 하는 중요한 포인트지점이다.

## 테스트 클래스
```java
public class main{
	public static void main(String[] args){
		Product com = ProductFactory.getProduct(new ComputerFactory("com1", 2000));
		Product tk = ProductFactory.getProduct(new TicketFactory("공연", 10000));
		System.out.println(com.toString());
        System.out.println(tk.toString());
	}
}
``` 


## Abstract Factory Pattern 사용의 장점
	1. 인터페이스 보다는 구조체에 접근할 수 있는 코드를 제공한다.
	2. 확장에 매우 용의한 패턴으로 쉽게 다른 서브 클래스들을 확장할 수 있다.
	3. 기존 Factory Pattern의 if-else로직에서 벗어날 수 있게 해준다.



