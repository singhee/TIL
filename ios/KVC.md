# [Objective-C] Key Value Coding (KVC)

`Key Value Coding` 은 어플리케이션이 정보를 의미하는 문자열(또는 키)를 사용하여 간접적으로 객체의 속성값을 접근하는 매커니즘을 말한다. Key-value Coding 은 Key-value Observing, Cocoa bindings, Core Data 와 함께 작업하는 기본적인 기술이다.

## 특징
- Key가 되는 문자열은 런타임시 결정된다.
- 소스 코드가 간결해지면서 유지보수도 쉬워진다.
- 클래스간 의존성이 낮아진다.

## Key-value Coding 사용 비교
### Key-value Coding을 사용하지 않은 경우
```objC
- (id)tableView:(NSTableView *)tableview
      objectValueForTableColumn:(id)column row:(NSInteger)row {

    ChildObject *child = [childrenArray objectAtIndex:row];
    if ([[column identifier] isEqualToString:@"name"]) {
        return [child name];
    }
    if ([[column identifier] isEqualToString:@"age"]) {
        return [child age];
    }
    if ([[column identifier] isEqualToString:@"favoriteColor"]) {
        return [child favoriteColor];
    }
    // And so on.
}
```
### Key-value Coding을 사용하는 경우
```objC
- (id)tableView:(NSTableView *)tableview
      objectValueForTableColumn:(id)column row:(NSInteger)row {

    ChildObject *child = [childrenArray objectAtIndex:row];
    return [child valueForKey:[column identifier]];
}
```

## 사용 함수
### 기본
일반적인 키에 대하 값을 얻을 때 사용한다.
- 값을 얻을 때 : `valueForKey:`
- 값을 설정할 때 : `setValue:forKey`
```objC
 NSLog(@"horsepower is %@", [engine valueForKey:@"horsepower"]);
 [engine setValue:[NSNumber numberWithInt:150] forKey:@"horsepower"];
```

### 키-경로
키-경로를 통해 속성 값을 얻을 때 사용한다.
- 값을 얻을 때 : `valueForKeyPath:`
- 값을 설정할 때 : `setValue:forKeyPath`
```objC
 NSLog(@“%@“, [selectedPerson valueForKeyPath:@"spouse.scooter.modelName”] );
```

### Array로 받기
키에 대한 값을 배열로 얻는다.
```objC
 NSArray *pressures = [car valueForKeyPath: @"tires.pressure”];
 NSLog (@"pressures %@", pressures);
```

### 원하는 키만 받기
원하는 키만 받을 때 사용한다.
```objC
 NSArray *keys = [NSArray arrayWithObjects:@"make", @"model",@"modelYear", nil];
 NSDictionary *carValues = [cardictionaryWithValuesForKeys:keys];
 NSLog(@"Car values : %@", carValues);
```


# Key Value Observing (KVO)

`Key Value Observing`은 모델 객체의 어떤 값이 변경되었을 경우 이를 UI에 반영하기 위해서 컨트롤러는 모델 객체에 Observing을 도입하여 델리게이트에 특정 메시지를 보내 처리할 수 있도록 하는 것이다.

## 특징
- 일대일, 일대다 관계에 대해서도 Observing을 적용할 수 있다.
- 모델 데이터에 반영되는 구조를 가진 앱은 코코아 바인딩을 사용하면 코드 작성을 최소화 할 수 있다.



