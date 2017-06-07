# Functional Programming & Filter, Reduce, Map

## 함수형 프로그래밍
간단하게 말하면, 함수형 프로그램은 수학 스타일의 함수와 불변성을 서술적인 표현식을 통한 연산에 중점을 두고 변수와 상태(state)의 사용을 최소화하는 프로그래밍 패러다임이다. 여기서 불변성이라 하면 변수와 상태(state)에 대비되는 의미로 상수와 스테이트리스(stateless)를 뜻한다고 보면 된다.

함수형 프로그래밍은 각 함수들간에 공유하는 상태를 최소화함으로써 함수에 대한 테스트가 용이하게 할 뿐만 아니라 `concurrency`한 병렬 처리에 적용하기 쉽기 때문에 인기를 얻어가고 있다. 이런 방법을 통해 최근의 멀티 코어 장비에서 성능을 향상시켜주는 한 가지 방법이 될 수 있다.

## 단순 배열 filter
다음은 1에서 10 사이의 짝수를 찾는 예제이다.

### 예전의 방식
```Swift 
var evens = [Int]()
for i in 1...10 {
  if i % 2 == 0 {
    evens.append(i)
  }
}
print(evens)  // 출력: [2, 4, 6, 8, 10]
```
이 예는 매우 단순한 내용이다. 1. 먼저 가변의(mutable) 빈 배열을 생성한다. 2. 1에서부터 10까지의 숫자에 대해 루프를 돈다. 3. 만약(`if`) 조건식(짝수이면)에 일치하면, 이 값을 배열에 추가한다.

이 예제코드 만으로도 원하는 결과를 잘 만들어낸다. 하지만 **숫자가 짝수인지 검사하는** 중요한 부분이 루프의 안에 숨어 있을 뿐만 아니라 배열에 숫자를 추가하는 부분도 조건식 안에 강하게 합쳐져(tight coupling) 있다. 앱의 다른 부분에서 각각의 짝수를 출력하려고 하면 코드를 복사/붙여넣기를 하지 않고는 코드를 재활용할 방법이 없다.

### 함수형 filter
위의 코드를 아래와 같이 고쳐서 실행하면 동일한 결과를 낸다.
```Swift 
func isEven(number: Int) -> Bool {
  return number % 2 == 0
}

var evens = [Int]()
evens = Array(1...10).filter(isEven)
print(evens) // 출력: [2, 4, 6, 8, 10]
```
위의 함수형 버전의 코드를 살펴보면 두 부분으로 이루어져 있다. 1. `Array(1...10)`은 1부터 10까지의 정수를 포함하는 배열을 간단히 생성하는 방법이다. 2. `filter`문이 함수형 프로그래밍 방법으로 일을 처리하는 부분이다. `filter`는 Array의 메서드로 주어진 함수가 true를 반환하는 아이템들만으로 구성되는 새로운 배열을 생성하여 반환한다. 여기서는 isEven 함수를 `filter`로 전달한다.

이 예제에서는 isEven 함수를 `filter`의 파라미터로 사용하였는데, 스위프트에서 함수는 이름이 있는 클로저의 특별한 형태라는 사실을 생각해보면 위의 코드는 아래와 같이 클로저를 사용하는 더욱 간결한 버전으로 만들 수 있다.
```Swift
var evens = [Int]()
evens = Array(1...10).filter { number in number % 2 == 0 }
print(evens) // 출력: [2, 4, 6, 8, 10]
```
위 예제에서는 컴파일러가 클로저의 파라미터와 리턴 값의 자료형을 문장의 컨텍스트를 통하여 유추하였다.

클로저의 문법에 따라 아래와 같이 더 간단한 방법으로 줄여 쓸 수도 있다.
```Swift
var evens = [Int]()
evens = Array(1...10).filter { $0 % 2 == 0 }
print(evens)
```
여기에서 함수형으로 작성된 코드가 최초의 예로든 예제 코드보다 훨씬 간결하다는 것을 알 수 있다. 이 간단한 예제를 통해서 모든 함수형 프로그램 언어의 흥미로운 공통된 특징을 찾아볼 수 있다.

```
1. 고차 함수 (Higher-order functions): 함수를 다른 함수의 파라미터로 전달할 수 있다. 예제 코드에서 filter에는 고차 함수를 전달 받도록 되어있다.
2. 일등 함수 (First-class functions): 함수를 변수처럼 다룰 수 있다. 함수를 변수에 대입하거나 다른 함수의 파라미터로 전달할 수 있다.
3. 클로저 (Closures): 익명 함수(이름이 지정되지 않은 함수)를 말한다.
```
Objective-C에서는 이런 기능들이 `블럭(block)`을 통해서 표현되었다. 스위프트에서는 Objective-C보다 더 간결한 문법으로 함수형 프로그래밍이 가능하도록 개선하였다.

### filter의 원리
스위프트 배열에는 `map`, `join`, `reduce`와 같은 몇 가지 함수형 메서드가 제공된다. 이런 함수들의 구현방법에 대해서 살펴 보도록 할것이다.
```Swift
func myFilter<T>(source: [T], predicate:(T) -> Bool) -> [T] {
  var result = [T]()
  for i in source {
    if predicate(i) {
      result.append(i)
    }
  }
  return result
}
```
이 예제의 함수는 T형 배열을 첫 번째 파라미터(source)로 받고, T형 파라미터를 받아 Bool을 반환하는 함수를 두 번째 파라미터(predicate)로 받아서 T형 배열을 반환하는 제네릭 함수이다.

이 함수 `myFilter`의 구현을 보면 우리가 최초로 작성한 일반적인 명령형 함수 버전과 매우 비슷하다. 이전에 하드 코딩으로 작성된 조건 검사 로직이 별도의 함수로 파라미터로 받는 다는 점이 가장 큰 차이점이다.

아래와 같은 코드로 `myFilter`를 호출해 보면 동일한 결과를 낸다는 것을 알 수 있다.
```Swift
evens = myFilter(Array(1...10)){ $0 % 2 == 0 }
print(evens) // 출력: [2, 4, 6, 8, 10]
```



[Swift Functional Programming Tutorial](http://www.raywenderlich.com/82599/swift-functional-programming-tutorial
)