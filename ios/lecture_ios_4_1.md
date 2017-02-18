# Stanford iOS 4강 - 화면의 기본, 뷰를 알아보자
## What is View?
1. View의 자식 뷰는 UIView의 subViews라는 변수로  찾아 볼 수 있다.  UIView의 배열로 저장이 되어있다. 
2. 뷰 계층은
1)  Xcode 내부에서 그래픽을 활용해서 만들어진다. 
2)  또한, 코드로 만들 수 있다.    
 —  `addSubView ()`
 —  `removeFromSuperView()`
3. ViewController 에는 최상위 계층에 있는 View의 포인터가 있다.  (여기서 말하는 최상의 계층 View는 bounds가 바뀌는 뷰를 말한다.)
-> `var view: UIView`
이 포인터를 이용하여 `addSubView()`나 `removeFromSuperView()`를 사용할 수 있다. 

## UIView의 생성
UIView를 위한 생성방식은 Initializer를 이용하는 방법과 ,  `awakeFromNib()`메소드를 사용하는 것이다.  Initializer를 이용하는 방법은 Code와 Storyboard 모두 사용가능하며, `awakeFromNib()`메소드를 호출하는 방법은 Storyboard에서만 유효한 방법이다. 
그렇다면 Initializer를 이용해 UIView를 생성하는 방법에 대해서 알아보도록 하자. UIView의 초기화는 조심해야한다. 중요한 생성자가 두 개 존재하기 때문이다.  

1) init (frame: CGRect)	//  뷰를 코드에서 만들 때 호출한다.
2) init (coder: NSCoder)	//  필수 생성자. Storyboard에서 UIView를 만들 때 사용된다.

만약 UIView의 초기화 과정에서 생성자가 필요하다면, 하나의 함수`setup()` 안에 모든 생성자를 넣어두는 것이 좋다. 그리고 이 2개의 생성자들을 override 해서 그 내부에서 `setup()`함수를 호출한다. 
```swift
func setup() { ... }

override init(frame: CGRect) {	// a designed initializer
	super.init(frame: frame)
	setup()
}

required init(coder aDecoder: NSCoder) {	// a required initializer
	super.init(coder: aDecoder)
	setup()
}
```

UIView를 `awakeFromNib()`메소드의 호출로 생성하는 방법에 대해서 알아보자. 앞서 말했듯이 이 방법은 스토리보드에서만 작동한다.  `awakeFromNib()`은 스토리보드에서 나오는 모든 객체에 호출할 수 있다.  생성할 View가 스토리보드에서 올 때만 동작하는 것이 괜찮다면, 위에서 본 setup() 함수에 넣어둔 것처럼 사용 가능하다. 

## Drawing
UIView위에 뭔가를 그리는 방법에 대해 공부하기 전에,  자료 타입에 대해 알아 보도록 하자.

### Coordinate System Data Structure 
먼저, 다음과 같이 드로잉 할 때 필요한 핵심 타입 4가지가 있다. 
1. CGFloat 
: Drawing 을 할 때는 Double로 계산한 값을 CGFloat로 바꿔야 한다.  
```swift
let cfg = CGFloat(aDouble) 
```
2. CGPoint
:  x좌표, y좌표 변수를 가지고 있어 하나의 점을 대변한다.
```swift
var point = CGPoing(x: 37.0, y: 55.2)
point.y -= 30
point.x += 20.0
```
3. CGSize
:  너비와 높이에 대한 두개의 변수를 가지고 있다.  
```swift
var size = CGSize(width: 100.0, height: 50.0)
size.width +=  42.5
size.height += 75
```
4. CGRect
:  CGPoint와 CGSize를 가지고 있다. 
```swift
struct CGRect {
	var origin: CGPoint
	var size: CGSize
}
let rect = CGRect(origin: aCGPoint, size: aCGSize)
```
 
이제는 뷰 안에서 드로잉을 할 좌표계에 대해서 알아보자. 
* Origin(원점)은 좌측 상단에 있다. 
* 드로잉에 사용되는 모든 단위들은 **points(포인트)**라고 불린다. 보통 iOS는 point당 2개의 pixel을 포함하고 있다. 
* pixel에 신경을 써야되는 상황이 있다면, UIView에게 contentScaleFactor가 무엇인지 물어야 한다.  이것은 포인트당 얼마나 많은 픽셀이 있는지를 말한다. 
```swift
var contentScaleFactor: CGFloat
```
* 드로잉의 boundary(경계선)







