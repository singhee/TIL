# UIGestureRecognizer
`UIGestureRecognizer`를 상속받아 사용할 수 있는 7가지 Gesture Recognizer가 있다.

* Tap gesture recognizer
* Swipe gesture recognizer
* Pan gesture recognizer
* Pinch gesture recognizer
* Rotation gesture recognizer
* Screen gesture recognizer
* LongPress gesture recognizer

## UITapGesuteRecognizer
`UITapGesuteRecognizer`는 다음과 같이 View에 추가할 수 있다.
```Swift
class ViewController: UIViewController {

	@IBOutlet var redView: UIView!

	override func viewDidLoad() {
		super.viewDidLoad()
		// Do any additional setup after loading the view, typically from a nib.

		var taps = UITapGestureRecognizer(target: self, action: Selector("handleTapGesture:"))
		self.redView.addGestureRecognizer(taps)
	}
}

extension ViewController {
	func handleTapGesture(recognizer: UITapGestureRecognizer) {
		println("Touch RedView")
	}
}
```

## UISwipeGestureRecognizer
`UISwipeGestureRecogizer`는 내가 원하는 Swipe 제스쳐에 대해서 만들어 줘야 한다. 예를 들면, Left, Right 제스쳐를 얻고자 한다면 하나의 `UISwipeGestureRecogizer`를 만들어 로직을 분리하는 것이 아니라 Left의 `UISwipeGestureRecogizer`와, Right의 `UISwipeGestureRecogizer`를 각각 만들어야 합니다.

다음은 `UISwipeGestureRecognizer`를 만들어 설정하는 코드이다.
```Swift
class ViewController: UIViewController {

	@IBOutlet var redView: UIView!

	override func viewDidLoad() {
		super.viewDidLoad()
		// Do any additional setup after loading the view, typically from a nib.

		var swipeRight = UISwipeGestureRecognizer(target: self, action: Selector("handleSwipeRightGesture:"))
		var swipeLeft = UISwipeGestureRecognizer(target: self, action: Selector("handleSwipeLeftGesture:"))
		var swipeUp = UISwipeGestureRecognizer(target: self, action: Selector("handleSwipeUpGesture:"))
		var swipeDown = UISwipeGestureRecognizer(target: self, action: Selector("handleSwipeDownGesture:"))

		swipeRight.direction = .Right
		swipeLeft.direction = .Left
		swipeUp.direction = .Up
		swipeDown.direction = .Down

		self.redView.gestureRecognizers = [swipeUp, swipeDown, swipeLeft, swipeRight]
	}
}

extension ViewController {
	func handleSwipeRightGesture(recognizer: UISwipeGestureRecognizer) {
		println("This swipe is right")
	}
	func handleSwipeLeftGesture(recognizer: UISwipeGestureRecognizer) {
		println("This swipe is left")
	}
	func handleSwipeUpGesture(recognizer: UISwipeGestureRecognizer) {
		println("This swipe is up")
	}
	func handleSwipeDownGesture(recognizer: UISwipeGestureRecognizer) {
		println("This swipe is down")
	}
}
```

## UIPanGestureRecognizer
`UIPanGestureRecognizer`에서 일반적인 표현으로 Drag 대신 Pan이라는 의미가 왜 사용되었는지 알 필요가 있다. *Panning*이라는 의미는 ‘카메라를 삼각대 위에 고정시켜 놓은 상태에서 움직이는 피사체를 따라 카메라를 수평으로 회전시키는 일’으로 디바이스는 고정되어 있는 상태에서 손가락이 움직이기 때문에 Pan이라는 단어를 사용한다. 

다음은 `UIPanGestureRecognizer`를 만들어 사용하는 코드이다.

```Swift
class ViewController: UIViewController {

	@IBOutlet var redView: UIView!

	override func viewDidLoad() {
		super.viewDidLoad()
		// Do any additional setup after loading the view, typically from a nib.
		var pan = UIPanGestureRecognizer(target: self, action: Selector("handlePanGesture:"))
		self.redView.addGestureRecognizer(pan)
	}
}

extension ViewController {
	func handlePanGesture(recognizer: UIPanGestureRecognizer) {
		var touchLocation = recognizer.locationInView(self.view)
		self.redView.center = touchLocation
		println(recognizer.translationInView(self.view))
	}
}
```
redView는 터치 좌표에 따라 중심이 이동한다. 또한, 최초의 터치 지점으로부터 얼마나 이동했는지 translationInView 함수를 통해 알 수 있다.

## UIPinchGestureRecognizer
`UIPinchGestureRecognizer`는 두 손가락을 이용하여 화면을 확대하거나 축소할 때 사용하는 경우가 많다.

다음은 특정 뷰 확대/축소를 사용하기 위한 코드이다.

```Swift
class ViewController: UIViewController {

	@IBOutlet var redView: UIView!

	override func viewDidLoad() {
		super.viewDidLoad()
		// Do any additional setup after loading the view, typically from a nib.
		var pinch = UIPinchGestureRecognizer(target: self, action: Selector("handlePinchGesture:"))
		self.redView.addGestureRecognizer(pinch)
	}
}

extension ViewController {
	func handlePinchGesture(recognizer: UIPinchGestureRecognizer) {
		if let pinchView = recognizer.view {
			pinchView.transform = CGAffineTransformScale(pinchView.transform, recognizer.scale, recognizer.scale)
			recognizer.scale = 1.0
		}
	}
}
```
뷰의 확대/축소를 하고자 한다면 recognizer의 scale를 1.0으로 복구해야 한다. 그렇지 않으면 핀치 줌을 이용하여 뷰를 확대/축소가 원하는 대로 되지 않는다.

확대/축소를 제한을 두고자 한다면 다음과 같이 작성할 수 있다.

```Swift
func handlePinchGesture(recognizer: UIPinchGestureRecognizer) {
	if recognizer.state == .Began {
		lastScale = recognizer.scale
	}
	if let pinchView = recognizer.view
		where recognizer.state == .Began || recognizer.state == .Changed
	{
		var currentScale = pinchView.layer.valueForKeyPath("transform.scale")?.floatValue
		let kMaxScale:CGFloat = 2.0
		let kMinScale:CGFloat = 0.7

		var newScale = 1.0 - (lastScale - recognizer.scale)
		if let currentScale = currentScale {
			newScale = min(newScale, kMaxScale / (CGFloat)(currentScale))
			newScale = max(newScale, kMinScale / (CGFloat)(currentScale))
			pinchView.transform = CGAffineTransformScale(pinchView.transform, newScale, newScale)
			recognizer.scale = 1.0				
			lastScale = recognizer.scale
		}
	}
}
```
최대, 최소의 크기를 정해놓고 Scale을 비교하여 최대, 최소 범위 내에 있는지 확인하고 transform의 scale를 정해준다.


## UIRotationGestureRecognizer
`UIRotationGestureRecognizer`는 두 손가락을 이용하여 뷰를 회전시킨다.

다음은 두 손가락을 이용하여 뷰를 회전시키는 코드이다.

```Swift
class ViewController: UIViewController {

	@IBOutlet var redView: UIView!

	override func viewDidLoad() {
		super.viewDidLoad()
		// Do any additional setup after loading the view, typically from a nib.
		var rotation = UIRotationGestureRecognizer(target: self, action: Selector("handleRotationGesture:"))
		self.redView.addGestureRecognizer(rotation)
	}
}

extension ViewController {
	func handleRotationGesture(recoginzer: UIRotationGestureRecognizer) {
		if let rotationView = recoginzer.view {
			rotationView.transform = CGAffineTransformRotate(rotationView.transform, recoginzer.rotation)
			recoginzer.rotation = 0.0
		}
	}
}
```

`UIRotationGestureRecognizer`의 rotation 값에 따라 뷰의 회전 값이 달라지게 된다.

## UIScreenEdgePanGestureRecognizer
`UIScreenEdgePanGestureRecognizer`는 스크린 모서리 근처에서 패닝하는 것을 찾는다. `UIScreenEdgePanGestureRecognizer`는 뷰에 붙이기 전에 방향을 설정해야 하며, 왼쪽, 오른쪽, 위, 아래를 설정할 수 있다.

다음은 스크린 왼쪽에서 패닝하는 것을 찾는 코드이다.

```Swift
class ViewController: UIViewController {

	@IBOutlet var redView: UIView!

	override func viewDidLoad() {
		super.viewDidLoad()
		var leftEdge = UIScreenEdgePanGestureRecognizer(target: self, action: Selector("handleLeftEdgeGesture:"))
		var rightEdge = UIScreenEdgePanGestureRecognizer(target: self, action: Selector("handleRightEdgeGesture:"))

		leftEdge.edges = UIRectEdge.Left
		rightEdge.edges = UIRectEdge.Right

		self.view.gestureRecognizers = [leftEdge, rightEdge]
	}

extension ViewController {
	func handleLeftEdgeGesture(recoginzer: UIScreenEdgePanGestureRecognizer) {
		println("This Edge is left")
	}
	func handleRightEdgeGesture(recoginzer: UIScreenEdgePanGestureRecognizer) {
		println("This Edge is right")
	}
}
```

## UILongPressGestureRecognizer
`UILongPressGestureRecognizer`는 버튼 또는 뷰 등을 오래 누르는 것을 찾는다. `UILongPressGestureRecognizer`는 `minimumPressDuration` 속성을 통해 특정 시간 후 이벤트를 받아오도록 설정하고, `allowableMovement` 속성을 통해 누르는 중에 얼마나 이동할 경우 실패할지 대해 설정한다.

다음은 특정 뷰를 오래 누르는 코드이다.

```Swift
class ViewController: UIViewController {

	@IBOutlet var redView: UIView!

	override func viewDidLoad() {
		super.viewDidLoad()
		var longPress = UILongPressGestureRecognizer(target: self, action: Selector("handleLongPressGesture:"))
		longPress.minimumPressDuration = 0.01
		self.redView.gestureRecognizers = [longPress]
	}

extension ViewController {
	func handleLongPressGesture(recogizer: UILongPressGestureRecognizer) {
		println("Now Finger is pressing")
	}
}
```

위의 코드에서 누른 후 0.01초 후에 이벤트가 발생한 것을 확인할 수 있다. 만일 1.0초 이후 이벤트가 시작하기를 원한다면 `minimumPressDuration`의 값을 1.0으로 설정하면 된다.

또한, 터치하다가 다른 곳으로 손가락이 이동한 경우 터치 좌표가 뷰에 내에 있는지 아닌지 판별해야 하는 경우가 있다. 이때 `CGRectContainsPoint` 함수를 이용하여 확인할 수 있다.

```Swift
func handleLongPressGesture(recogizer: UILongPressGestureRecognizer) {
	var p = recogizer.locationInView(recogizer.view?.superview)
	if let lView = recogizer.view
		where recogizer.state == .Changed {
		if CGRectContainsPoint(lView.frame, p) {
			// Touch Point is inner
		} else {
			// Touch Point is outer
		}
	}
}
```
만약 꾹 누르다가 해당 `UILongPressGestureRecognizer`를 더이상 안받고자 하는 경우 기존 `UILongPressGestureRecognizer`를 제거하고 다시 추가하면 다시 `UILongPressGestureRecognizer`를 시작할 수 있다.

```Swift
extension ViewController {
	func handleLongPressGesture(recogizer: UILongPressGestureRecognizer) {
		println(__FUNCTION__)
		var p = recogizer.locationInView(recogizer.view?.superview)
		let state = recogizer.state
		if let lView = recogizer.view
			where recogizer.state == .Changed
		{
			if CGRectContainsPoint(lView.frame, p) {
				// Touch Point is inner
				count++
			} else {
				// Touch Point is outer
				count++
			}

			if count > 10 {
				println("Remove Gesture")
				count = 0
				var longPress = UILongPressGestureRecognizer(target: self, action: Selector("handleLongPressGesture:"))
				longPress.minimumPressDuration = 0.01
				lView.removeGestureRecognizer(recogizer)
				lView.addGestureRecognizer(longPress)
			}
		} else if (state == .Ended || state == .Cancelled || state == .Failed || state == .Began) {
			count = 0
		}
	}
}
```
위 코드를 좀 더 확장하여 특정 화면을 특정 시간까지 눌러야 하는 경우, 터치 시작할 때 타이머를 생성하여 특정 시간이 되면 지정된 동작을 수행하거나, 터치 지점이 뷰를 벗어나면 타이머를 취소하도록 할 수 있다.

다음은 터치 시작시 타이머 동작하여 특정 시간이 되면 지정된 함수를 수행하는 코드이다.
첫번째로, 타이머 속성과 남은 시간 확인하는 클로저를 가지는 속성을 정의한다.

```Swift
let kTimer = 20

class ViewController: UIViewController {

	@IBOutlet var redView: UIView!

	var pressedTouchTimer: NSTimer?
	var elapsedHandler: (Void -> Bool)?

	override func viewDidLoad() {
		super.viewDidLoad()
	}
}
```

다음으로 `UILongPressGestureRecognizer`를 생성하여 붙일 함수를 만든다.
```Swift
func attachLongPressGesture() {
	self.stopTimer()
	let longPress = UILongPressGestureRecognizer(target: self, action: Selector("handleLongPressGesture:"))
	longPress.minimumPressDuration = 0.01
	self.redView.gestureRecognizers = [longPress]
}
```

다음은 클로저를 이용하여 특정 값에 도달하면 true/false를 반환하는 함수를 만든다.
```Swift
func makeElapsedTime() -> ( Void -> Bool) {
	var elapsedTime = kTimer
	return {
        if --elapsedTime < 0 { elapsedTime = 0 }
        return elapsedTime == 0 ? true : false
    }
}

다음은 터치 시작할 때 동작할 타이머와 타이머가 호출될 때 실행될 함수를 만든다.

```Swift
func startTimer() {
	self.elapsedHandler = makeElapsedTime()

	self.pressedTouchTimer = NSTimer.scheduledTimerWithTimeInterval(
		0.1,
		target: self,
		selector: Selector("longPressTimerHandler"),
		userInfo: nil,
		repeats: true)
}

func stopTimer() {
	self.pressedTouchTimer?.invalidate()
	self.pressedTouchTimer = nil
	self.elapsedHandler = nil
}

// 지정된 시간이 지날경우 타이머를 종료하고 UILongPressGestureRecognizer를 초기화 하여 더이상 처리하지 않도록 한다.
func longPressTimerHandler() {
	guard let isElapsedTime = self.elapsedHandler where isElapsedTime() else { return }
    self.attachLongPressGesture()
}
```

이제 `UILongPressGestureRecognizer`를 처리하는 함수를 만든다. 여기에서 터치 시작할 때 타이머를 설정하고 제어한다.
```Swift
class ViewController: UIViewController {

	@IBOutlet var redView: UIView!

	var pressedTouchTimer: NSTimer?
	var elapsedHandler: (Void -> Bool)?

	override func viewDidLoad() {
		super.viewDidLoad()

		self.attachLongPressGesture()
	}
	deinit {
		self.stopTimer()
	}

}

extension ViewController 
{
	func handleLongPressGesture(recogizer: UILongPressGestureRecognizer) {
	    switch recogizer.state {
	    case .Began:
	        self.startTimer()
	    case .Ended, .Cancelled, .Failed:
	        self.stopTimer()
	    case .Changed:
	        guard let lView = recogizer.view else { return }
	        let location = recogizer.locationInView(lView.superview)
	        if !CGRectContainsPoint(lView.frame, location) {
	            self.attachLongPressGesture()
	        }
	    default: break
	    }
	}
}
```

---------------------------------------------------------------
[http://minsone.github.io/mac/ios/uigesturerecognizer](http://minsone.github.io/mac/ios/uigesturerecognizer)


