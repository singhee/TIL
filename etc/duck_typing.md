# Duck Typing
	"만약 어떤 새가 오리처럼 걷고, 헤엄치고, 꽥꽥거리는 소리를 낸다면 나는 그 새를 오리라고 부를 것이다" - 다형성(상속)

	동적 타이핑, 인스턴스의 실제 타입에 상관 없이 어떠한 동작을 할 수 있는지를 확인하여 타입을 판단한다.


```objc
#import <Foundation/Foundation.h>

@interface Car : NSObject
- (void)go;
@end
@implementation Car
- (void)go {
    NSLog(@"Car go");
}
@end

@interface Truck : NSObject
- (void)go;
@end
@implementation Truck
- (void)go {
    NSLog(@"Truck go");
}
@end

// Duck Typing
void foo(id p)
{
    [p go];
}


int main()
{
    foo([Car new]);     // Car go
    foo([Truck new]);	// Truck go
}

```

