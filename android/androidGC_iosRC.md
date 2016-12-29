# Android Garbage Collection vs iOS Reference Counting
`Garbage Collection`과 `Reference Counting` 모두 사용하지 않는 객체를 메모리에서 해제하기 위핞 수단이다. 하지만 동작하는 방식에 차이가 있고, 이는 두 운영체제의 설계 의도를 반영하는 듯 하다.

## Garbage Collection
`Garbage Collection`은 **Mark-and-sweep**알고리즘을 기반으로 동작한다. Garbage Collector가 GC root를 순회하면서 참조할 수 있는 모든 오브젝트를 마킹한다. 이후 불특정한 시간에 마킹되지 않은 모든 오브젝트가 메모리로부터 수거된다. 
`Garbage Collection`의 단점은 언제 garbage가 수거되는지 예측할 수 없다는 것이다. 

## Reference Counting 
`Reference Counting`은 각 오브젝트에 대한 Automatic Ref Couning을 수행하는 방식으로, 참조되는 수가 0이 되면 해당 오브젝트가 메모리로부터 제거된다. GC가 중앙집중적인 관리라면 RC는 분산적인 관리라고 할 수 있다. 

단말 성능이 많이 좋아진 지금은 RC가 아닌 GC를 사용하기 때문에 안드로이드 사용성이 별로라고 할 수는 없다. 

`Reference Counting`의 단점은 순환 참조(circular dependency)가 생겼을 때 오브젝트들이 메모리에 그대로 남아 메모리 누수 현상이 발생하는 것이다. 물론 안드로이드에서도 메모리 누수는 발생하지만 순환 참조가 생겨날 가능성에 대해 iOS에서만큼 개발자들이 고민하지 않아도 된다. 왜냐하면 순환 참조에 포함된 오브젝트들이 모두 GC root로 부터 참조되지 않고 있다면 Collector가 이들을 수거해 갈 수 있기 떄문에, 복잡한 어플리케이션을 제작할 때 개발자들이 비즈니스 로직에 더 집중할 수 있도록 도와주는 셈이다. 

안드로이드는 "general purpose computing"를 위한것이니 만큼, 복잡하고 큰 규모의 어플리케이션 개발도 지원할 수 있도록 다양한 도구를 제공한다고 한다. 그에 비해 iOS는 부드럽고 매끄러운 사용자 경험을 최우선으로 하는데, GC와 RC의 동작방식이 그러한 설계 의도를 반영하는 것 처럼 보인다고 한다.