# [Xcode] 스토리보드, 어떻게 깔끔하게 사용할까?

## 요점
스토리보드가 복잡할때, 깔끔하게 사용/관리하는 방법.
스토리보드를 나누어서 사용하자.
이니셜라이징 코드를 뷰컨트롤러 코드안에 선언해주자.
세그웨이를 이용하여 push, present 하는 것 보다, 코드를 이용해 이니셜라이징한 뷰컨트롤러를 push, present 하자.

## 1. 팀 작업인 경우, 스토리보드를 나누어서 사용하자. (개인 작업이라도 스토리보드를 나누는 방법은 매우 좋은 습관이 될 것이다.)

[Figure_1. storyboard](../images/storyboard.png)

프로젝트에서 `main.storyboard`하나만 쓰는 경우는 디자이너 관점에서 보면, 뷰의 전체흐름을 볼 수 있어서 좋아 보인다. 그렇지만, 개발자에게 몇가지 문제를 일으킬 수 있다.

- 소스관리 : 스토리보드 수정 후, 머지할 때 컨플릭트가 나기쉽고, 이것이 생각보다 간단하게 풀리지 않을 때가 있다.(스토리보드는 xml형식으로 작성된다.) 
- 스토리 보드파일이 무거워지고, 네비게이팅 하기가 점점 힘들어진다.
- 클릭을 잘못해서 constraint를 바꾸는 경우도 종종 있다.
- 모든 뷰 컨트롤러 별로 스토리보드 아이디를 할당해 주어야 한다. 직접 할당하다보면 실수 할 여지가 있다.

따라서, 스토리보드는 분리해야 한다. 그렇다면, 스토리보드간 연결은 어떻게 해주어야 할까? 다음의 두 가지 방법이 있다. 

1. 스토리보드 wrapper 를 하는 방법
2. 스토리보드를 코드로 연결하는 방법

### 스토리보드를 코드로 연결하기
#### 1. 뷰 컨트롤러 이름과 스토리보드 파일 이름을 같도록 맞춘다.
이것은 나중에 도움이 많이 되는 네이밍 컨벤션이다.

#### 2. 뷰 컨트롤러에서 스토리보드 이니셜라이징 한다.
보통 스토리보드를 이용한 뷰 컨트롤러 이니셜라이징을 할 때, 아래와 같은 코드를 자주 접하게 된다.
```swift
let storyboard = UIStoryboard(name: "Main", bundle: nil)
let homeViewController = storyboard.instantiateViewController(withIdentifier: "HomeViewController")
```
위의 코드는 아주 깨끗한 코드는 아니다. 왜냐하면, 스토리보드 명도 필요하고, 해당하는 뷰 컨트롤러에 스토리보드ID도 설정해야하며, 이러한 과정을 매번 homeViewController 를 만들 때 해줘야 한다.
더 좋은 방법은 **위의 코드를 뷰 컨트롤러 안에 옮겨놓고, static method 를 이용해서 스토리보드와 함께 이니셜라이징 하는 방법이다.**

```swift
class HomeViewController: UIViewController {
	static func storyboardInstance() -> HomeViewController? {
		let storyboard = UIStoryboard(name: "HomeViewController", bundle: nil)
		return storyboard.instantiateInitialViewController() as? HomeViewController
	}
}
```
그리고 난 후 아래와 같이 스토리보드 아이디를 클래스 이름으로 설정하도록 한다. 이 방법을 쓰면 하드코딩으로 스토리보드 아이디 설정하는 방법을 피할 수 있고, 대신 클래스명을 직접 이용해서 사용할 수 있어서 에러를 줄일 수 있다.
```swift
extension NSObject {
	static func classNameToString() -> String {
		return String(reflecting: type(of: self)).components(separatedBy: ".").last!
	}

	func classNameToString() -> String {
		return String(reflecting: type(of: self)).components(separatedBy: ".").last!
	}
}

class HomeViewController: UIViewController {
	static func storyboardInstance() -> HomeViewController? {
		let storyboard = UIStoryboard(name: classNameToString(), bundle: nil)
		return storyboard.instantiateInitialViewController() as? HomeViewController
	}
}
```

이제부터는 뷰 컨트롤러를 이니셜라이즈 할 때, 한줄로 만들수 있게 된다.
```swift
let homeViewController = HomeViewController.storyboardInstance()
```

#### 3. 스토리보드 세그웨이관련 메소드를 오버라이딩 하지 않는다.
만약 싱글 스토리보드 안에 여러 뷰 컨트롤러가 있어도 세그웨이 관련 메소드를 오버라이드 하지 않는게 좋다. 왜냐하면
- 각 세그웨이 마다 이름을 달아야 하며, 이런것은 에러 유발 가능성이 있다. 하드코딩하는 것은 좋은 프로그래밍 습관이 아니다.
- `prepareForSegue` method 는 세그웨이가 추가 될때마다 점점 못생겨지고 읽기 어려운 코딩이 될 것이다.

그럼 어떻게 해야되나? 예를 들어 버튼 클릭시 다음 뷰 컨트롤러로 push 할 때는 단순히 IBAction을 걸고, 이 메소드 안에서 뷰 컨트롤러 이니셜라이징 코드를 넣으면 된다.
```swift
@IBAction func didTapHomeButton(_ sender: AnyObject) {
	if let homeViewController = HomeViewController.storyboardInstance() {
		navigationController?.pushViewController(homeViewController, animated: true)
	}
}
```



