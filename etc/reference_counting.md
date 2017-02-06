# Reference Counting 
Reference Counting 은 객체의 수명을 관리하는 기술로 객체의 메모리를 참조하는 계수를 기반으로 수명을 관리한다.

## Reference Counting 의 등장 이유
Reference Counting 을 이해하기 위해서는 먼저, ++복사정책++에 대해 알아야 한다. 복사 정책은 객체가 가진 값 형식(Value Type)과 참조형식(Reference Type)의 복제 방식에 따라 두 가지로 나누어진다.
    
    1. 깊은 복사 (deep copy)
    2. 참조 복사 (shallow copy)

두 가지 이외에도 복사 금지, 소유권 이전(move)라는 개념도 있지만 위의 두 가지 정책에 대해서 집중적으로 살펴보도롣 하겠다. 

### 1. 깊은 복사 (deep copy)
깊은 복사는 C++ 의 복사 정책으로, 복합객체를 새롭게 생성합니다. 그러기 때문에 처음에 만들었던 객체와 복사된 객체가 서로 별개의 객체가 되기 때문에 (서로 다른 참조를 가지고 있기 때문에)어느 한 쪽을 수정한다고 해서 다른 한쪽이 영향 받는 일이 없다. 이는 깊은 복사의 장점이라 할 수 있는 ++안정성++을 뜻한다. 하지만, 복사하려는 객체가 커진다면 복사 시 성능저하 문제나, 불필요한 중복이 발생할 수 있다는 단점이 있다. 

### 2. 참조 복사 (얕은 복사, shallow copy)
참조 복사는 Java 의 복사 정책으로, 참조값(Reference Type)을 복사하는 것을 말한다. 즉, 같은 참조를 가지게 된다. 

> Java 의 깊은 복사
Java 에서도 깊은 복사가 가능하다. 이는 사용자가 정의한 클래스를 Cloneable interface 를 implement 하여 clone() 메소드를 오버라이드 함으로써 구현 가능하다. 

참조 복사는 같은 참조를 가지게 되므로 복사에 대한 비용이 들지 않는다는 장점이 있다. 하지만, 다른 레퍼런스를 통한 변경으로 인한 버그 문제로 안전하지 않다는 단점이 있다. 이는 멀티 스레드 환경에서 더더욱 큰 문제가 될 수 있다. 이를 해결하기 위해 나온 개념이 바로 불변객체(Immutable Object)이다.

하지만, 불변객체로 참조 복사의 문제를 모두 해결한 것은 아니다. 참조 복사는 두 객체가 독립적이지 못하게 하기 때문에 한 객체에서 포인터가 가리키는 값을 바꾸게 되면 다른 객체에서도 영향을 받는다. 만약, 이 상황에서 한 객체에서 포인터를 삭제를 하면 다른 객체는 유효하지 않는 메모리 영역을 가리키고 있게 되는 문제가 생긴다. 이러한 문제를 `댕글링 포인터(Dangling Pointer)`라 한다. 

Reference Counting 은 바로 이 `댕글링 포인터(Dangling Pointer)` 의 해결을 위해 나온 개념인 것이다. 참조 계수를 기반으로 하여 객체의 수명 관리를 하는 것이다. Reference Counting 에는 다음과 같이 두 가지가 있다. 

    1. ARC 
    2. GC


#### 1. ARC (Auto Reference Counting)
	컴파일러가 컴파일 타입에 코드를 분석해서 참조계수를 조작하는 코드를 삽입한다.
	"ObjC, Swift"


###### > ObjC 로 ARC 기능 구현
```objc
#import <Foundation/Foundation.h>
// .h
@interface Image : NSObject



- (id)init;
- (void)dealloc;
@end

// .m
@implementation Image

- (id)init {
    self = [super init];
    if (self) {
        NSLog(@"객체 생성");
    }
    
    return self;
}

- (void)dealloc {
    NSLog(@"객체 파괴");
    
    [super dealloc];
}
@end


int main()
{
    //  1. 객체가 처음 생성되면 ref counting 1 입니다.
    // Image p1 = new Image();
    Image* p1 = [Image new];
    NSLog(@"ref count: %ld", [p1 retainCount]);
    
    // 2. 객체의 포인터를 대입하면 참조 계수를 증가해야 합니다.
    Image* p2 = p1;
    [p2 retain];
    NSLog(@"ref count: %ld", [p1 retainCount]);
    
    // 3. 더 이상 사용하지 않는다면, 참조 계수를 감소해야 합니다.
    [p1 release];
    [p2 release];
    
    NSLog(@"프로그램 종료");
}

```	


#### 2. GC (Garbage Collector)
	실행시간에 Garbage Collector 가 주기적으로 Reference Count 가 0인 객체를 수거한다.
	"JAVA, C#"

-------
#### 약한참조와 강한참조
```objc
#import <Foundation/Foundation.h>

@interface Node : NSObject
@property(weak) Node* next;  // 혹은 @property(strong) Node* next;
// weak: auto niling => 참조하는 객체가 파괴되면, 자동으로 next를 nil로 바꿔준다. / 약한 참조
// strong: 단순 참조만을하고 있을때도 레퍼런스 카운트가 감소되지 않는다./ 강한 참조
@end

@implementation Node
- (void)dealloc {
NSLog(@"Node 파괴");
}
@end

// 참조 계수 기반의 객체 수명 관리에서는 순환 참조에 의한 메모리 누수가 발생한다.
// 참조 계수를 무작정 증가시키는 것은 위험하다.
//  => 소유권
//     '있다' => 강한 참조 (생성 책임, 소멸 책임)
//     '없다' => 약한 참조 (참조만을 목적으로 한다)
void foo()
{
    Node* p1 = [Node new];
    Node* p2 = [Node new];

    for (int i = 0 ; i< 10000 ; ++i) {
        p1.next = p2;
        p2.next = p1;
    }
}


int main()
{
    NSDate* p = [NSDate date];
    foo();
    NSLog(@"Time: %lf", -[p timeIntervalSinceNow]);

}
```

#### 약한 참조 
	1. SoftReference - OutOfMemory 발생 시 수거
	2. Weak Reference - GarbageCollector -> Reference Count 가 0일 때 수거

