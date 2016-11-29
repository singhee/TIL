# Java Reflection

Reflection (Instropection) : Class 이름만으로 클래스의 정보(필드, 메소드)를 찾거나 객체를 생성할 수 있는 기능이다.

자바는 스크립트 언어가 아닌 컴파일 언어이다. 자바에서는 동적으로 객체를 생성하는 기술이 없었다. 그리고 동적으로 인스턴스를 생성하는 Reflection으로 그 역활을 대신하게 된다.

```
Instances of the class Class represent classes and interfaces in a running Java application. 
Every array also belongs to a class that is reflected as a Class object that is shared by all arrays with the same element type and number of dimensions. 
The primitive Java types (boolean, byte, char, short, int, long,float, and double), 
and the keyword void are also represented as Class objects.

Class has no public constructor. Instead Class objects are constructed automatically by the Java Virtual Machine as classes are loaded and by calls to the defineClass method in the class loader.
```

간단하게 살펴보면 Class객체는 java에서 사용되는 모든 class와 interface.array등을 표현할 수 있으며 Class객체는 생성자를 포함하고 있지 않는다. Class객체에서는 class의 내부정보(class명, 생성자, 메소드, 멤버변수)등을 표시하는 메소들을 제공하는데 이를 담기위한 그릇의 형태로 Reflection API가 사용된다. Class에서 제공되는 메소드를 이용하여 Reflection API에 값을 담아 표시하는데 주로 사용되는 Class메소드-Reflection API은 밑에서 설명하도록 하겠다.

### Java Reflection의 사용
Reflection에는 3가지 형태가 있다.
첫번째로는 `getClass()`를 이용해 Class를 얻는 것이다. 두 번째는 `Modifier` 클래스의 `getModifiers()`를 통해 Class의 속성을 얻는 것이다. 마지막으로는 Method 속성을 얻는 형태로 `getMethod()`를 통해 얻는다.

Reflection의 활용은 동적으로 메소드를 호출할 때 나타난다. Reflection의 성능은 염려할 만큼의 성능저하를 가져오지 않는다. 또한 잘 설계된 Reflection은 객체 지향 철학을 어기지 않으면서도 더 좋은 성능을 발휘할 수도 있으며, 잘 설계된 Reflection이 제공하는 서비스는 개발을 더욱 단순화 한다. 