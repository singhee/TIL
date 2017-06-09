# Swift Any vs AnyObject
Swift에서는 Int, String, Any, Class Type등 일반적인 타입으로 선언되는 변수/상수의 경우 컴파일러에서 엄격하게 타입에 대한 검사를 한다. 이것들은 선언할 때 즉시 타입이 확정되며 이후 다른 타입의 값을 대입이 허용되지 않는다.

그런데 이런 정적인 타입 외에도 가변 타입을 사용해 유연하게 사용할 수 있는 방법 역시 제공하는데 바로 `Any`과 `AnyObject`이다. (Any는 AnyObject를 포함한다.)

```
AnyObject: 모든 클래스 타입의 인스턴스를 AnyObject로 표현 가능
Any: 모든 타입의 인스턴스를 Any로 표현 가능(function 타입 포함)
```

```Swift
var anyArray: [Any] = [0, "str", true, 9.9, aClass]
var anyObjectArray: [AnyObject] = [aClass, bClass, view1, viewController]
```
위에서 보는 바와 같이 원래 Array는 Int를 Int, String은 String으로 동일한 타입만 저장할 수 있게 되어있으나 Any를 사용하여 다양한 타입을 사용할 수 있게 된다.

그런데 이렇게 Any, AnyObject를 사용하게 되면 가변 타입이므로, 각 타입이 가진 고유의 프로퍼티나 메소드를 바로 사용할 수가 없기에 타입 체크 또는 타입 캐스팅이 필요하게 된다. 


