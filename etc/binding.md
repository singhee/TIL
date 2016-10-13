## binding

##### binding : 어떤 메소드를 호출할지 결정하는 매커니즘
- static binding: compile-time
	a 가 어떤 타입인지를 보고 결정한다.
	early binding, 빠르다
	C++, C#
	
	`virtual`(메소드의 binding: 정적 -> 동적)



- dynamic binding: run-time
	a 가 실제로 가지고 있는 객체의 타입으로 결정한다.
	late binding, 느리다. 이성적으로 동작한다.
	Java, ObjC


```java
class Program
{
    class Animal
    {
        public virtual void Cry() { Console.WriteLine("Animal cry");  }  // 1
    }

    class Dog : Animal
    {
        public override void Cry() { Console.WriteLine("Dog cry"); }     // 2
    }

    class Puddle : Dog
    {
        public override void Cry()
        {
            Console.WriteLine("Puddle Cry");
        }
    }

    class Cat : Animal
    {
        public override void Cry()
        {
            Console.WriteLine("Cat cry");
        }
    }

    static void Main(string[] args)
    {
        Animal a = new Animal();
        a.Cry();        // 1
        a = new Dog();          // Dog is a Animal
                                // 자식 객체를 부모의 레퍼런스에 저장할 수 있다.

        a.Cry();        // 2

        // Animal a = new Animal();
        // a.Cry();    // ?

        // Dog d = new Dog();
        // d.Cry();    // ?
    }
}

```