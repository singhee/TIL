# Reference Counting 
Reference Counting 은 객체의 수명을 관리하는 기술로 객체의 메모리를 참조하는 계수를 기반으로 수명을 관리한다.

	1. ARC 
	2. GC

## Reference Counting 의 등장 이유
Reference Counting 을 이해하기 위해서는 먼저, ++복사정책++에 대해 알아야 한다. 복사 정책에는 두 가지가 있다. 
    
    1. 깊은 복사
    2. 얕은 복사 

두 가지 이외에도 복사 금지, 소유권 이전(move)라는 개념도 있지만 위의 두 가지 정책에 대해서 집중적으로 살펴보도롣 하겠다. 

### 1. 깊은 복사 
깊은 복사는 C++ 의 복사 정책으로, 


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

