# Swift Access Level (접근 한정자)
Swift 3 의 접근 한정자(Access Level)는 5가지가 존재한다. 각각의 접근 한정자에 대해 알아보자.

### `open`
가장 개방된 접근 한정자로써 소속 모듈 또는 소속 모듈을 import하는 모든 모듈에서 class와 class 멤버에 접근할 수 있으며 open class를 상속 받아 sub class를 생성하거나 메서드를 override 할 수 있다. 간단히 이야기 하면 다른 언어에서의 public과 유사하다.

### `public`
`open` 과 동일한 접근을 허용하지만, sub class의 생성과 override에 제한이 있다. 소속 모듈 내에서는 sub class 생성과 sub class 내에서의 override가 허용된다. 이 제한은 프레임워크(모듈)을 제작하는 경우 유용하다. 프레임워크 내에서는 자유롭게 상속 받지만 외부에서는 상속을 받을 수 없기 때문에 확장을 제한할 수 있다.

### `internal`
접근 한정자가 지정되지 않은 경우 기본적으로 사용되는 접근 수준이다. 소속 모듈의 모든 소스 파일에서 사용할 수 있지만 모듈 외부에서는 접근 할 수 없다.

### `fileprivate`
소속 소스 파일 내에서만 접근이 가능하다. 

### `private`
현재 소스를 둘러싸는 선언으로 내에서만 접근 가능하다.

Swift 3에서는 위에서 열거한 5가지 레벨로 접근을 제한하도록 되어 있다. 단, Objective-C 클래스와 메소드는 이제 `open` 상태로 가져온다.

접근 한정자의 사용이 복잡하게 생각될 수도 있지만, 어플리케이션은 하나의 모듈과 동일하게 생각하면 되므로 어플리케이션 내에서 작성된 코드는 기본적으로 모든 어플리케이션 소스 내에서 접근 가능하다. 따라서 프레임워크(모듈)를 제작하는 것이 아니라면 `fileprivate` 과 `private` 를 이용해서 접근을 제한하는 경우만 고려하면 된다.


## fileprivate 및 private의 사용
소스 파일 외부에서 액세스 하지 않으려는 속성이 있는 ViewController를 작성한다고 가정하자. Swift 2에서는 다음과 같이 `private`로 선언할 것이다.
```Swift
class RootViewController: UIViewController {
  private var someFlag = false
}
```

하지만 Swift 3를 사용한다면, 같은 소스 파일에서 class를 확장하면서 `someFlag`에 접근하려 하면 오류가 발생한다.
```Swift
extension RootViewController: MyGreatDelegate {
  func doSomething {
    if someFlag {
      // do the thing
    }
  }
}

// Use of unresolved identifier 'someFlag'
```
Swift 3에서 `private`는 `someFlag`를 둘러싼 class의 선언 내로 제한되기 때문에 오류가 발생하는 것이다. 따라서 이런 경우에는 아래와 같이 `fileprivate` 를 사용해야 한다.
```Swift
class RootViewController: UIViewController {
  fileprivate var someFlag = false
}
```
아래는 class를 확장하면서 확장 외부에서 접근하지 못하는 메서드를 정의하는 예시다.
```
extension RootViewController: UITextFieldDelegate {
  func textFieldShouldReturn(_ textField: UITextField) -> Bool {
    handle(text: textField.text)
    textField.resignFirstResponder()
    return true
  }

  // Not accessible outside of this extension
  private func handle(text: String?) {
    // do something
  }
}
```
[ttps://blog.asamaru.net/2017/01/04/swift-3-access-controls/](https://blog.asamaru.net/2017/01/04/swift-3-access-controls/)