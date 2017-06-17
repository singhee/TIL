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