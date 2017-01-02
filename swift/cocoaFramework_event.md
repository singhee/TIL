# Cocoa Framework 에서 이벤트를 처리하는 방법 3가지
**1. Target-Action : Selector를 이용하는 방법**
**2. Delegate**
**3. Closure**

## 1. Target-Action
- Handler 기반의 이벤트 처리 방식
- UIButton
- NotificationCenter
- 문제점 : ARC와 사용할 경우 레퍼런스 카운팅을 제대로 적용할 수 없는 경우가 발생할 수 있다. 
-> 곧deprecated될 가능성이 크다.

```swift
class Button: NSObject {
	var target: AnyObjecy?
	var action: Seletor?

	func click() {
		_ = target?.perform(action)
	}

	func add(_ target: AnyObject, action: Selector) {
		self.target = target
		self.action = action
	}
}

class Dialog: NSObject {
	func close() {
		print("Dialog close")
	}
}

let dialog = Dialog()
let button = Button()

var a = #selector(Dialog.close)

button.add(dialog, action: a)
button.click()
```

## 2. Delegate
- 인터페이스 기반의 이벤트 처리 방식
- 어디로부터 이벤트가 발생하였는지 정보가 반드시 필요하다.
- 단점 : 프로토콜을 준수해야 한다.

```swift
protocol ButtonDelegate: class {
    func onClick(_ sender: AnyObject)
}


class Button: NSObject {
    weak var delegate: ButtonDelegate?
    
    func click() {
        delegate?.onClick(self)
    }
}

class Dialog: NSObject, ButtonDelegate {
    func onClick(_ sender: AnyObject) {
        close()
    }
    
    func close() {
        print("close")
    }
}

let button = Button()
let dialog = Dialog()

button.delegate = dialog
button.click()
```

## 3. Closure
- UIAlertController
- 함수형 언어의 특징

```swift
class Button: NSObject {
	var handler: (()->Void)?

	func click() {
		self.handler?()
	}

	func setHandler(handler: @escaping (()->Void)) {
		self.handler = handler
	}
}

class Dialog: NSObject {
	func close() {
		print("close")
	}
}

let dialog = Dialog()
let button = Button()

button.setHandler {
	dialog.close()
}

button.click()
```
