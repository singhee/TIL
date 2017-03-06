# [Swift] Dictinary vs [ObjC] NSDictionary
프로그래밍에서 **딕셔너리**는 키워드와 그 정의를 쌍으로 가지고 있는 컬렉션이다. 

## Swift - Dictionary
Swift의 딕셔너리는 요소들이 순서 없이 키와 값의 쌍으로 구성되는 컬렉션 타입이다. 

## ObjC - NSDictionary
코코아는 딕셔너리의 역할을 하는 `NSDictionary`라는 컬렉션 클래스를 가지고 있다. `NSDictionary` 는 주어진 키에 맞는 값(보통 `NSString`, 어떤 종류의 객체도 될 수 있다.) 을 저장한다. 그러면 대응되는 값을 찾기 위해 키를 사용할 수 있다. 

`NSDictionary`는 `NSString`과 `NSArray` 처럼 Immutable 객체이다. 그러나 내용을 추가하고 제거할 수 있도록 해주는 `NSMutableDictionary` 클래스를 제공한다. 

`NSDictionary`의 중간에는 `nil` 값을 저장할 수 없다. 
