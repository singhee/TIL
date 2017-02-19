# Stanford iOS 4강
## 화면의 기본, 뷰를 알아보자
1. View의 자식 뷰는 UIView의 subViews라는 변수로  찾아 볼 수 있다.  UIView의 배열로 저장이 되어있다. 

2. 뷰 계층은
1)  Xcode 내부에서 그래픽을 활용해서 만들어진다.
2)  또한, 코드로 만들 수 있다.    
* `addSubView ()`
* `removeFromSuperView()`

3. ViewController 에는 최상위 계층에 있는 View의 포인터가 있다.  (여기서 말하는 최상의 계층 View는 bounds가 바뀌는 뷰를 말한다.)
: `var view: UIView` 이 포인터를 이용하여 `addSubView()`나 `removeFromSuperView()`를 사용할 수 있다. 

### UIView의 생성
UIView를 위한 생성방식은 Initializer를 이용하는 방법과 ,`awakeFromNib()`메소드를 사용하는 것이다.  Initializer를 이용하는 방법은 Code와 Storyboard 모두 사용가능하며, `awakeFromNib()`메소드를 호출하는 방법은 Storyboard에서만 유효한 방법이다. 
그렇다면 Initializer를 이용해 UIView를 생성하는 방법에 대해서 알아보도록 하자. UIView의 초기화는 조심해야한다. 중요한 생성자가 두 개 존재하기 때문이다.  

1) init (frame: CGRect)	: 뷰를 코드에서 만들 때 호출한다.
2) init (coder: NSCoder) : 필수 생성자. Storyboard에서 UIView를 만들 때 사용된다.

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

### Drawing
UIView위에 뭔가를 그리는 방법에 대해 공부하기 전에,  자료 타입에 대해 알아 보도록 하자.

### Coordinate System Data Structure 
먼저, 다음과 같이 드로잉 할 때 필요한 핵심 타입 4가지가 있다. 

1. CGFloat : Drawing 을 할 때는 Double로 계산한 값을 CGFloat로 바꿔야 한다.  
```swift
let cfg = CGFloat(aDouble) 
```

2. CGRect :  x좌표, y좌표 변수를 가지고 있어 하나의 점을 대변한다.
```swift
var point = CGPoing(x: 37.0, y: 55.2)
point.y -= 30
point.x += 20.0
```

3. CGSize :  너비와 높이에 대한 두개의 변수를 가지고 있다.  
```swift
var size = CGSize(width: 100.0, height: 50.0)
size.width +=  42.5
size.height += 75
```

4. CGRect :  CGPoint와 CGSize를 가지고 있다. 
```swift
struct CGRect {
	var origin: CGPoint
	var size: CGSize
}
let rect = CGRect(origin: aCGPoint, size: aCGSize)
```

자, 이제는 뷰 안에서 드로잉을 할 좌표계에 대해서 알아보자. 
* Origin(원점)은 좌측 상단에 있다. 
* 드로잉에 사용되는 모든 단위들은 **points(포인트)**라고 불린다. 보통 iOS는 point당 2개의 pixel을 포함하고 있다. 
* pixel에 신경을 써야되는 상황이 있다면, UIView에게 contentScaleFactor가 무엇인지 물어야 한다.  이것은 포인트당 얼마나 많은 픽셀이 있는지를 말한다. 
```swift
var contentScaleFactor: CGFloat
```
* 드로잉의 boundary(경계선)


## 뷰는 어떻게 만들까 

### Creating Views
대부분의 뷰는 스토리보드에서 드래그 해오는 방식으로 만든다. 하지만, Custom View 는 스토리보드에서 드래그 하여 가져올 수 없다. 이는 일반화된 뷰(어떠한 상속도 받지 않은 UIView)를 **identiry inspector**에서 수정하여 UIView의 자식클래스로 만듦으로써 해결할 수 있다. 
UIView를 Code로 생성하는 것은 드문 일이지만, 만약 Code로 생성해야 하는 일이 생긴다면, UIView의 생성자를 호출하는 방법으로 생성한다. 이때, 인자로 coder를 받는게 아니라 frame 을 인자로 받아야 한다.  아래의 코드는 UIView를 생성하는 기본 코드이다. 
```swift
let labelRect = CGRect(x:20, y:20, width: 100, height: 50)
let label = UILabel(frame: labelRect) // UILabel is a subclass of UIView
label.text = "Hello"
view.addSubview(label)
```

### Custom Views
* 왜 UIView의 자식뷰인 커스텀 뷰를 만드는 것일까?
draw를 하기 위해서는 UIView를 상속받은 다음 `drawRect()`메소드 하나만 Override 해주면 그릴 수 있다.  **단, 절대로 `drawRect()`메소드를 호출해서는 안된다.**
* 뷰를 다시 그리는 방법
`setNeedsDisplay()`메소드는 “이 뷰는 다시 그려질 필요가 있다”는 것을 시스템에게 알려준다.  그렇다면 뷰는 적절한 시점에 `drawRect()` 를 호출할 것이다.
`setNeedsDisplayInRect()`메소드는 인자로 받는 직사각형 부분만 그려주는 메소드이다. --> `setNeedsDisplay()`의 최적화된 버전
* `drawRect()`는 어떻게 실행할까
1)  C언어 같은 방법 : Core Graphics (전역 함수로 처리하는 방법)
2)  객체지향 방법: UIBezierPath 클래스를 사용하는 방법 (UIBezierPath도 결국 밑단에서는 Core Graphics를 사용하는 것)

> **Core Graphics?**
> Core Graphics 는 iOS에서 화면에 그리는 기본적인 개념을 말한다. Core Graphics의 개념은 다음과 같다. 
> 1. 그릴 수 있는 Context가 필요하다. (`UIGraphicsGetCurrentContext()`)
> 2. 경로(path)를 만든다. - 호나, 직선, 직사각형, 원 등으로 되어 있다.
> 3.  경로를 그리는 데 필요한 속성들을 설정 해준다.  (ex. colors, fonts, textures, linecaps)
> 4.  Stroke or fill - 경로를 그어주거나, 경로로 닫힌 영역에 정해진 색이나 텍스쳐로 채운다. 


> ** UIBezierPath?**
> UIBezierPath 클래스도 모든 걸 같은 방식으로 처리한다. 다만, Core Graphics와 다른 점은 자동으로 Context를 처리할 수 있다는 점이다. UIBezierPath는 Core Graphics 내용 모두를 하나의 클래스로 캡슐화한 개념이라고 생각하면 된다. 색, 폰트, 이미지를 제외하고는 필요한 모든 것을 UIBezierPath를 통해서 그릴 수 있다. ( 색은 UIColor 가 한다.)

## 그림을 그려보자 
### Defining a Path 
UIBezierPath에 대해 알아보았으니, 이제는 Path(경로)를 생성하고 무언인가를 그리기 위한 방법에 대해 알아보도록 하자. 삼각형을 그려 볼 것이다.
* UIBezierPath 생성하기
```swift
let path = UIBezierPath()	 // UIBezierPath 생성. 
							 // 빈 Path를 만들기 위해 빈 생성자 호출
path.moveToPoint(CGPoint(80, 50)) // 점 좌표 옮기기 - 시작점
path.addLineToPoint(CGPoint(140, 150))	// 선 추가하기
path.addLineToPoint(CGPoint(10, 150))	// 선 추가하기 
path.closePath()		// path 닫기 - 첫 시작점으로 돌아가 경로를 닫는다
```
만약, 위의 코드를 drawRect 에 넣더라도 그림이 화면에 나타나지는 않을 것이다.  위의 과정은 path를 생성한 것일 뿐이다. 실제로는 아직 획을 긋거나(stroke) 채우지 (fill)않은 것이다.  통해 화면에 보여주기 위해서는 drawRect에 획을 긋거나 채울 색상을 설정하는 코드를 추가해주면 된다.
```swift
UIColor.greenColor().setFill()	
UIColor.redColor().setStroke()
path.linewidth = 3.0
path.fill() 	// 선 안쪽으로 초록색이 채워진다.
path.stroke() // 바깥면에 빨간선이 생긴다. 
```

### Drawing 
* UIBezierPath의 다양한 초기화함수를 사용하면 좀 더 복잡한 것을 그릴 수도 있다.  
```swift
let roundRect = UIBezierPath(roundedRect: aCGRect, cornerRadius: aCGFloat)
let oval = UIBezierPath(ovalInRect: aCGRect)
```
* UIBezierPath는 **clipping path(경로를 깍아내기)**로 설정할 수도 있다. : `addClip()`
* UIBezierPath는 부딛힘을 감지한다. : `func containsPoint(CGPoint) -> Bool`

### UIColor
* Color에 대한것은 모두 UIColor의 type method를 이용한다. 
```swift
let green = UIColor.greenColor() 
```

> `greenColor()`는 UIColor 클래스 안에 `static Class func greenColor()`라고 정의되어 있다. 물론, 색상은 RGB, HSB를 통해 원하는 색상을 만들 수도 있으며 UIImage를 이용하여 pattern 을 만들기도 가능하다.

* UIColor에서는 투명도를 설정할 수 있다. 
```swift
let transparentYellow = UIColor.yellowColor().colorWithAlphaComponent(0.5)
// colorWithAlphaComponent 메소드는 인스턴스 메소드이다. 
```
만약 뷰에서 투명도가 들어간 것을 그리게 된다면, 뷰 안에서 `opaque`변수를 `false`로 설정해 주어야 한다. 이는 성능 최적화를 위해서 시스템이 모든 뷰가 불투명할 것이라고 가정하기 때문이다. 
재밌는 점은 전체 뷰의 `alpha`프로퍼티를 설정하게 되면 전체 뷰 또한 투명하게 만들수 있다. `alpha`를 설정하는 것은 뷰를 점차 희미해지게 하는 것 같은 효과를 주는데 유용하다.  

### View Transparency
하위뷰의 리스트는 하위뷰를 추가해가면서 만들어지는데, 즉, `addSubView()`를 할 때 앞에 놓여지게 된다.   `insertSubViewAt()` 원하는 위치에 subView를 추가해 준다.  이렇게 하위뷰간의 순서는 매우 중요하다. 그 순서로 View에 보여지기 때문이다. 
뷰를 완전히 숨길수도 있는데, 이 때에는 `hidden`속성을 `true`로 설정해주면 된다.  `hidden`속성이 `true`로 설정되면 뷰 계층에는 여전히 들어가 있지만 화면에 보이지도 않고, 어떠한 터치 이벤트도 받지 않는다.  이 경우는 어떤 특정 상태일 때만 보이는 뷰가 필요할 때 사용한다. 



## 텍스트와 이미지를 넣으려면 
### Drawing Text 
Text는 UIBezierPath로 그리지 않는다. Text는 폰트로 표현되는 UIBezierPath의 모음일 뿐이다.  폰트는 UIBezierPath의 모음으로 된 각각의 문자를 정의한다. 문자는 UILabel을 사용해서 그려낸다. 하지만, drawRect를 이용해 텍스트를 그리고 싶다면 어떨까? 그럴 땐 `NSAttributedString`클래스를 사용한다.  이는 다음과 같은 구조로 생성 가능하다.
```swift
let text = NSAttributedString("hello")
text.drawAtPoint(aCGPoint)
let textSize: CGSize = text.size
```
AttributedString은 각각의 문자를 어떻게 그릴지에 대한 속성(색상, 글꼴, 배경색상 등)으로 구성된 Dictionary형태로 되어있다. AttributedString이 작동하는 기본적인 방식은 다음과 같다. 
* 문자에 대한 Dictionary를 설정한 후 
* drawRect 안에서 그려준다.

AttributedString은 Objective-C 클래스이기 때문에 사용하기에 좋지 않은 점이 몇 개 있다. 아직 스위프트로는 좋은 성능이 나오지 않고 있기 때문이다. 스위프트에서 AttributedString을 사용하는 것을 복잡하게 만드는 주요한 두가지 차이점은 다음과 같다. 
* 1.  immutable 하다. 만약 mutable한 string을 만들기 위해서는 `NSMutableAttributedString`이라는 다른 클래스를 사용해야 한다. 
```swift
let mutableText = NSMutableAttributedString("some string")
```

> NSAttributedString 은 Swift의 String도 아니고, Objective-C 의 NSString도 아닌 별개의 것이다. string과 mutableString 이라는 프로퍼티가 있는데 bridging(연결)을통해 String이나 NSString으로 돌려줄 수 있다.  하지만, 그 자체로는 String 은 아니다. 

* 2.  범위에 대한 것이다. AttributedString의 attribute를 설정하려면 다음과 같이 하면 된다. 
```swift
func setAttributes(attributes: Dictionary, range: NSRange)
func addAttributes(attributes: Dictionary, range: NSRange)
```
위에서 볼 수 있는 NSRange 는 Range와는 다르다. NSRange안에는 index가 있고,  범위를 표현하는 구조체로 되어있다.  index는 또한 Objective-C의 NSString에 있다. 

> Objective-C의 NSString vs Swift의 String
> Swift의 String은 완전히 유니코드를 따른다. 따라서, NSString의 문자길이는 String의 문자길이와 다를수도 있다.  반면,  NSString은 유니코드 전부를 반드시 다루지 않을수도 있다.  그러기 때문에 range에 대해서는 조심히 확인할 필요가 있다. 

잠시 다른 얘기를 해보도록 하자. 위에서 AttributedString의 attributes를 설정하기 위해 Dictionary를 전달해야 했다. 이 Dictionary 에 넣어야 되는 속성들에는 무엇이 있을까?
```swift
// Dictionary 속성
NSForegroundColorAttributeName: UIColor	// 텍스트 색깔
NSStrokeWidthAttributeName: CGFloat	// 텍스트 두께 
NSFontAttributeName: UIFont	// 텍스트 폰트
```


### Fonts
iOS에서 Font는 매우 중요하다. iOS를 보고 느끼는데 매우 중요한 역할을 하기 때문이다. 이러한 폰트를 내가 원하는 폰트로 선택해 가져올 수 있는 방법은 무엇일까?
폰트를 가져오는 방법은 3가지가 있다.
첫번째 방법은 `preferredFont`를 사용하는 것이다.  `preferredFont`는 사용자의 컨텐츠(날씨앱의 온도, 캘린더 앱의 일정)를 보여줄 때 사용하는 폰트이다. 
```swift
static func preferredFontForTextStyle(UIFontTextStyle) -> UIFont	// code로 폰트 적용 시킬 때 
```

두번째 방법은 `systemFont`를 사용하는 것이다.  `systemFont`는 버튼같은 시스템 요소에 사용된다. 
```swift
static func systemFontOfSize(pointSize: CGFloat) -> UIFont
static func boldSystemFontOfSize(pointSize: CGFloat) -> UIFont
```

세번째 방법은 특정한 폰트를 쓰는 방법으로,  커스터마이징한 폰트를 사용하는 것이다. 

### Drawing Images
텍스트를 그리기 위해 UILabel이 있던 것처럼 Image를 그리기 위해서는 UIImageView가 필요하다.  UIImageView를 먼저 추가한 뒤에 그 위에 Image를 집어넣는 것이다. 하지만, UIImage를 drawRect안에 그려넣고 싶다면 다음과 같은 구조로 그려줄 수 있다.
```swift
// 1. UIImage를 가져온다 
// 가져오는 방식은 1) 파일 이름으로 가져오거나 2) Images.xcassets에 이미지를 넣어 놓고 이름을 준 다음, UIImage(named: 이미지 이름) 로 가져온다. 단, 이는 이미지를 가져오는데 실패할 수 있기 때문에 옵셔널이다. 
let image: UIImage? = UIImage(named: "foo")	

// 2. drawRect안에 속성을 지정해주는 코드를 더한다.
image.drawAtPoint(aCGPoint)
image.drawInRect(aCGPoint)
image.drawAsPatternInRect(aCGPoint)
```

### Redraw on bounds change?
bounds가 바뀌었을 때 다시 그려야 되는 상황이 온다면  UIView의 contentMode라는 프로퍼티를 이용한다.  contentMode의 값으로 Content를 새 bounds의 장소로 이동시키는 것이다.  contentMode의 값으로 설정할 수 있는 값은 다음과 같다. 
* 위치 이동에 관한 값 
: `.Left` / `.Right` / `.Top `/ `.Bottom` / `.TopRight `/ `.TopLeft `/ `.BottomLeft` /`.Center`
* 크기 조정에 관한 값
:` .ScaleToFill`(default),  `.ScaleAspectFill`, `.ScaleAspectFit`
* 다시 그리기 - drawRect() 호출
: `.Redraw`
 





