# Swift 3.0의 Range에 대해
Swift 3.0이 나오면서 Range에 대한 것이 좀 많이 바뀌었다. 이에 따라 Xcode 8 Beta6기준으로 확인해 보았다.

* Proposal: A New Model for Collections and Indices
* 단힌 가격 구현
* 끝의 값은 Bound에서 얻음
* 같은 형명(Rnage)에서도 Swift 3.0에서는 for-in등으로 사용할 수 없음

## 닫힌 간격
Swift 2.x의 Range는 열린 구간만의 표현으로 되어 있지만, Swift 3.0에서는 닫힌 간격의 구현이 추가되었다.
```Swift
let x = 1...10
```
위 코드에서 초기화하면 아래와 같은 차이가  있다. Swift 3.0부터는 닫힌 간격이 구현된 사양 변경에 따라 startIndex, endIndex실행형태가 바뀌었다. 이전까지는 닫힌구간 사이에서도 열린 구간에서도 Range<T>의 T는 얻을수 있었지만, Swift 3.0에서 ClosedRangeIndex<T>라는 형태의 구조로 리턴된다.

* 형: Range<Int> => CountableClosedRange<Int>
* 값: 1..<11 => 1…10
* count: 10 => 10
* startIndex: 1 => ClosedRangeIndex<T>
* endIndex: 11 => ClosedRangeIndex<T>
* upperBound: – => 1
* lowerBound: – => 10

## 열린 구간
```Swift
let x = 1..<10
```
열린 구간은 형의 변경만으로 초기화된 값이나 얻을 수 있는 값을 변경하지 않는다.

* 형: Range<Int> => CountableClosedRange<Int>
* 값: 1..<10 => 1..<10
* count: 9 => 9
* startIndex: 1 => 1
* endIndex: 10 => 10
* upperBound: – => 1
* lowerBound: – => 10

## Countable이 아닌 Range
```Swift
let x = ClosedRange(uncheckedBounds: (1, 10))
let y = Range(uncheckedBounds: (1, 10))
```
주의해야할 점은 같은 형명도 Swift 3.0의 Range가 Sequence프로토콜을 지키지 않도록 되어 있어 for-in과 forEach에서 사용할 수 없게 되어 있다.

[출처](https://swifter.kr/2016/09/01/swift-3-0의-range에-대해/)

